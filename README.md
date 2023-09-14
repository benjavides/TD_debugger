> This is a working proof of concept for debugging python code executing inside of Touchdesigner. Can trigger breakpoints and watch the execution state (inspect variables). Tested in Touchdesigner 2022.31030.

![gif](./assets/gif.gif)

# 🛠 Setup

📽 You can alternatively follow [this video tutorial](https://youtu.be/Dc1tJpdgRW8).

1. 🐍 Install `debugpy` in Touchdesigner's python installation:

   1. In Touchdesigner's textport execute:

      ```python
      print(sys.exec_prefix)
      ```

   2. 📂 Go to the path in the output and open a `cmd` console there. Run:

      ```shell
      python -m pip install debugpy
      ```

   3. ❗ If there is an error saying `No module named pip`, you can use [this solution](https://stackoverflow.com/questions/9780717/bash-pip-command-not-found/57666133#57666133). Translated to Windows it would be:

      1. 🌐 Go to [the link](https://bootstrap.pypa.io/get-pip.py) in a browser and save the file (`ctrl+s`).

      2. 📂 Put it in the path where the `python.exe` is located (output of step 1.1).

      3. ⌨️ On a command prompt located there run:

         ```shell
         arduinoCopy code
         python get-pip.py
         ```

      4. 🔄 Restart the console and try step 1.2 again.

2. 🛣️ Touchdesigner paths have to be replicated in Windows file system for breakpoints set in the UI to trigger:

   1. 📂 Put the `TD_debugger` folder directly in your C drive.
   2. 📂 Put the `.vscode` inside `TD_debugger` directly in your C drive.

   You can skip step 2 but you are going to have to set the breakpoints explicitly in the code with the command `debugpy.breakpoint()`. If you choose to use it like this, make sure that the pathMappings section in your launch.json is like this:

   ```
   "pathMappings": [{
                   "localRoot": "${workspaceFolder}",
                   "remoteRoot": "c:"
               }],
   ```

   Where the letter of the `remoteRoot` has to match the drive letter of where you have the `Debugger.toe`.

3. On a terminal run

   ```
   setx DEBUGPY_PROCESS_SPAWN_TIMEOUT 90
   ```

   to increase the time vscode is going to wait for the debugger to connect to the server. Thanks to [@AlphaMoonbaseBerlin](https://github.com/AlphaMoonbaseBerlin) for figuring this out!

4. 🖥️ Open Visual Studio Code in your C drive.

5. 🎨 Open the `Debugger.toe` in TouchDesigner.

6. 🕹️ Connect vscode by going to the Debug tab and pressing the play button. A dropdown should open, look for `TouchDesigner.exe` and select that process. It takes around 35 seconds for the debugger to connect the first time.

🔍 If the bottom bar turns orange it means that it was able to connect successfully. Add a breakpoint in line 15 in vscode and trigger a Jump in the exampleExtension using the jump pulse. Vs code should stop at that line and allow you to inspect the state of the program in the left column.



# 🤔 How does this work?

I think that vscode injects a dll in the running Touchdesigner process which launches a debugpy server to which the vscode debugger connects to. By default it times out, that is why we have to increase the timeout time



# 📝 To do

-  🚀Figure out how to make the breakopoints set in the vscode UI work with this. I think the problem is related to the paths because when we move the whole project directly to the C drive and the paths are equal in Touchdesigner and in Windows, it works.

[Link to discussion](https://forum.derivative.ca/t/python-debugger-profiler/298670/15) on Derivative's forum