# s21-boeing

Technical Documentation

Main Menu Scripts:

ChangeScene.cs -

This script is used on the scene changing buttons of the Main Menu’s tablet to load the user into their selected scene. If the user hits the “Main Menu” button, the function “ChangeToMenu()” is called to send them to the main menu. If the user hits the “SandboxTutorial” button, the function “ChangeToSandbox()” is called to send them to the sandbox scene. Lastly, if the user hits the “Tooling Practice” button, the function “ChangeToTooling()” is called to send the user to the ToolingEnvo scene.
 
Sandbox Tutorial Scripts:

TaskCycle.cs - 

TaskCycle.cs is the script that handles the task rotation accessed by the “Task” button of the SandboxTutorial’s tablet. This window shows the user every task available to them, allows them to cycle to each task, provides a complete/incomplete button, and provides a “Back” button to return to the menu. The script only handles which task is shown to the user and the buttons that transition between tasks.
To start, the script initializes the task number to “1”, activates task number 1, and deactivates the rest of the tasks.
 
Upon the user hitting either the “back” button or the “next” button, the script will then decrement the task number by 1 for hitting back, or increment the task number by 1 for hitting next. If the value of task number goes below 1, task number will be manually set to 6, the last task page, for cycling purposes. The same goes for the task number going above 6, the task number will be set to 1, the first task page, for cycling purposes. Each time the user hits back or next, the script will first deactivate the currently shown task window and then activate the new task window along with any buttons necessary for its functionality.
   
