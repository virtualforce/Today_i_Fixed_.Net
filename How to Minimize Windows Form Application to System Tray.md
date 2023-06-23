# How to Minimize Windows Form Application to System Tray?

# Problem
Almost all the Windows Form apps come with the functionality to minimize the application to the taskbar as usual routine. This is perfect for applications that are used frequently and require rapid access. What happens, though, if we need to keep a program running in the background that is only used occasionally? If we minimize such an app toward the taskbar, it fills up the taskbar, and the taskbar becomes cluttered with open programs that are difficult to navigate. For this purpose, we can minimize the application to the system tray. The system tray is located at the bottom-right corner of the taskbar.

# Environment
Visual Studio, Asp.Net , C#

# How you fix it
By following the steps given in the solution below:

# Solution 
In order to use the system tray, we utilized `NotifyIcon` control under the namespace called `System.Windows.Forms`.

Some important `NotifyIcon` properties are as follows:

- **BalloonTipIcon:** indicates the icon which will appear with the balloon tip.
- **BalloonTipText:** It is the text that will appear in the balloon tip.
- **BalloonTipTitle:** It is the title given to balloon tip.
- **Icon:** It is the most important characteristic of the symbol that will be visible in the system tray. Only.ico files are supported.
- **Text:** It is the text that appears when you move your cursor over the icon in the system tray.
- **Visible:** It is the property to set the icon's visibility in the system tray.

Some properties that we will set in our application are as follows.

````c#
private void Form1_Load(object sender, EventArgs e)
		{
			notifyIcon1.BalloonTipTitle = "System Tray";
			notifyIcon1.BalloonTipText = "Some Notification";
			notifyIcon1.Text = "System Tray App";

		}
````



# Minimize Windows Form Application to System Tray

 Here is a step-by-step procedure for minimizing the application on the system tray instead of the taskbar.

Steps: -

1. Open the visual studio. After launching the visual studio.

2. Select `Create a new project` from the menu as shown in the picture below.
3. Create a Windows form application by selecting `WindowsFormApp` from the menu as shown in the application. Then click on the next button.
4. Finally, we type the name of the Window Form application and click on the create button as shown in the figure below.
5. The very first task after building a standard Windows Forms application is to take the `NotifyIcon` from the toolbar and put it onto the form.
6. After doing this, open the property window of `NotifyIcon` and set the visible state to `False`. 
7. Also, select an icon for your application that will display on the system tray. If you did not select an icon for your application then you will not see the application in the system tray after minimizing it.
8. Now add the following code in the `NotifyIcon` in the `MouseDoubleClick` Method.

````c#
private void notifyIcon1_MouseDoubleClick(object sender, MouseEventArgs e)
		{
			this.Show();
			notifyIcon1.Visible=false;
			WindowState = FormWindowState.Normal;
		}
````

9. Now our next step is to add an event to our form. In order to do this, double-click on the form and select the resize event from the event menu.

10. Now add the following code to activate the event.

````c#
private void Form1_Resize(object sender, EventArgs e)
		{
			if (WindowState == FormWindowState.Minimized)
			{
				this.Hide();
				notifyIcon1.Visible = true;
				notifyIcon1.ShowBalloonTip(1000);
			}
			else if (FormWindowState.Normal == this.WindowState)
			{
				notifyIcon1.Visible = false;
			}
		}
````

11. Also, add the following code in the `Form1_Load` method to set the various properties of `NotifyIcon`.

````c#
private void Form1_Load(object sender, EventArgs e)
		{
			notifyIcon1.BalloonTipTitle = "System Tray";
			notifyIcon1.BalloonTipText = "Some Notification";
			notifyIcon1.Text = "System Tray App";

		}
````

12. We have done now let's test our application.
13. when we will click the minimize button on the form as shown below. Then the Application will be minimized in the system tray.
# Conclusion

In general, almost all the applications are minimized on the taskbar and due to this behavior of the application, conflicts will produce while opening or closing the application. 

That is why It is important to minimize the sensitive applications to the system tray so that we can avoid conflicts. So, we can easily change the behavior of a typical program/Application that may be affected by the system tray instead of the taskbar while minimizing as described above.

The major advantage of the system tray is that it is utilized with a tiny icon that really does not obstruct the user's view. Still, the program is indeed easily reachable. That is why applications that do their functions in the background should be placed in the System tray.


