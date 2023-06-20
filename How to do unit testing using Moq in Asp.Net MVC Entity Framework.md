# How to do unit testing using Moq in Asp.Net MVC Entity Framework?

# Problem
How to do unit testing in .Net without relying on dependencies such as databases, external services, or components? 

# Environment
Visual Studio, Asp.Net MVC, C#

# How you fix it
By following the steps given in the solution below:

# Solution 
You can do the unit testing of an application without relying on dependencies such as databases, external services, 
or components by following these steps
* Right click on sloution >> Add >> Test >> Unit Test Project >> give it name and click create.
* After creating the project, you must install the Moq library from the NuGet packages manager.
* Then add a reference of your application in the Unit test project.
Suppose we have the following controller in our application and want to do its unit testing.
```
namespace FirstCrust2._0.Controllers
{
    public class CategoriesController : Controller
    {
        public ApplicationDbContext db = new ApplicationDbContext();
        // GET: Categories
        public ActionResult Index()
        {
            return View(db.Category.ToList());
        }

        // GET: Categories/Details/5
        public ActionResult Details(int? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            Category category = db.Category.Find(id);
            if (category == null)
            {
                return HttpNotFound();
            }
            return View(category);
        }
    }
}
```
In order to do unit testing of the above controller we need to write seprate tests for each of Action methods in the class of unit test project. As shown below
```
namespace FirstCRUSTTest
{
    [TestClass]
    public class CategoryTest
    {

        // Arrang
        private List<Category> GetFakeEmployeeList()
        {
            return new List<Category>
            {
                new Category { CreatedBy = "Adnan", ID = 1, IsActive=true, ModifiedBy="Irfan", Name="Cake" },
                new Category { CreatedBy = "Ali", ID = 2, IsActive = true, ModifiedBy = "Ashraf", Name = "Biscuts" },
                    // Add more categories as needed
            };    
        }
        [TestMethod]
        public void Index()
        {
            // Arrange
            var categories = new List<Category>
            {
                new Category { CreatedBy = "Adnan", ID = 1, IsActive=true, ModifiedBy="Habib", Name="Cake" },
                new Category { CreatedBy = "Ali", ID = 2, IsActive = true, ModifiedBy = "Zara", Name = "Biscuts" }
            };
            var mockSet = new Mock<DbSet<Category>>();
            mockSet.As<IQueryable<Category>>().Setup(m => m.Provider).Returns(categories.AsQueryable().Provider);
            mockSet.As<IQueryable<Category>>().Setup(m => m.Expression).Returns(categories.AsQueryable().Expression);
            mockSet.As<IQueryable<Category>>().Setup(m => m.ElementType).Returns(categories.AsQueryable().ElementType);
            mockSet.As<IQueryable<Category>>().Setup(m => m.GetEnumerator()).Returns(categories.AsQueryable().GetEnumerator());

            var mockContext = new Mock<ApplicationDbContext>();
            mockContext.Setup(db => db.Category).Returns(mockSet.Object);

            var controller = new CategoriesController();
            controller.db = mockContext.Object;

            // Act
            var result = controller.Index() as ViewResult;

            // Assert
            Assert.IsNotNull(result);
            Assert.AreEqual(categories.Count, (result.Model as List<Category>).Count);
        }
        [TestMethod]
        public void Details()
        {
            // Arrange
            
            int categoryId = 1;
            var category = new Category { ID = 1, Name = "Cake" };
            var mockSet = new Mock<DbSet<Category>>();
            mockSet.Setup(m => m.Find(categoryId)).Returns(category);
            var mockContext = new Mock<ApplicationDbContext>();
            mockContext.Setup(db => db.Category).Returns(mockSet.Object);
            var controller = new CategoriesController();
            controller.db = mockContext.Object;

            // Act
            var result = controller.Details(categoryId) as ViewResult;

            // Assert
            Assert.IsNotNull(result);
            Assert.AreEqual(category, result.Model);
        }

        [TestMethod]
        public void Details_NullId_ReturnsBadRequest()
        {
            // Arrange
            int? categoryId = null;
            var controller = new CategoriesController();

            // Act
            var result = controller.Details(categoryId) as HttpStatusCodeResult;

            // Assert
            Assert.IsNotNull(result);
            Assert.AreEqual(HttpStatusCode.BadRequest, (HttpStatusCode)result.StatusCode);
        }

        [TestMethod]
        public void Details_InvalidId_ReturnsHttpNotFound()
        {
            // Arrange
            int categoryId = 999;
            var mockSet = new Mock<DbSet<Category>>();
            mockSet.Setup(m => m.Find(categoryId)).Returns((Category)null);
            var mockContext = new Mock<ApplicationDbContext>();
            mockContext.Setup(db => db.Category).Returns(mockSet.Object);
            var controller = new CategoriesController();
            controller.db = mockContext.Object;

            // Act
            var result = controller.Details(categoryId) as HttpNotFoundResult;

            // Assert
            Assert.IsNotNull(result);
        }
        [TestMethod]
        public void Create()
        {
            // Arrange
            var controller = new CategoriesController();

            // Act
            var result = controller.Create() as ViewResult;

            // Assert
            Assert.IsNotNull(result);
        }
    }
}
```
* Now you can simply Run the All the tests by selecting Test button from the title bar.
