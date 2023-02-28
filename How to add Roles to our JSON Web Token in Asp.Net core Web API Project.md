# How to add Roles to our JSON Web Token in Asp.Net core Web API Project

# Problem
While implementing authentication we can also define user roles so only a user or number of users with the speicific role can access some paticular API.

# Environment
Visual Studio, Asp.Net Core, C#

# How you fix it
We can define claims while creating JWT.

# Solution
We have to define our role while creating the JWT using the claims property provided to us by JWT.
```
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, "username"),
    new Claim(ClaimTypes.Role, "admin")
};

var key = new SymmetricSecurityKey(Encoding.UTF8.GetBytes("your-secret-key"));
var credentials = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);

var token = new JwtSecurityToken(
    issuer: "your-issuer",
    audience: "your-audience",
    claims: claims,
    expires: DateTime.UtcNow.AddMinutes(30),
    signingCredentials: credentials);

var tokenString = new JwtSecurityTokenHandler().WriteToken(token);
```
Here we are adding the admin role in our Claims.

Now we have to define our role while using the authorize attribute so the api will only allow the user having admin role in its JWT.
```
[HttpGet]
[Authorize(Roles = "admin")]
public IActionResult Get()
{
    // return data accessible only to "admin" role
}
```
