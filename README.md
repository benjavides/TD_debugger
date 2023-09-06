> This is a working proof of concept for debugging python code executing inside of Touchdesigner

# Setup

1. Install `debugpy` in Touchdesigner's python installation

   1. In Touchdesigner's textport execute:

      ```
      print(sys.exec_prefix)
      ```

   2. Go to the path in the output and open a `cmd` console there. Run

      ```
      python -m pip install debugpy
      ```

2. Touchdesigner paths have to be replicated in Windows file system
   1. Put the `TD_debugger` folder directly in your C drive
   2. Put the `.vscode` inside `TD_debugger` directly in your C drive

3. Open Visual Studio Code in your C drive.

4. Open Touchdesigner. Go to `TD_debugger/exampleExtension` and in the custom  parameters press `Init Debugger` to launch our own debugpy server. Wait for it to time out and throw an error.

5. Connect vscode by going to the Debug tab and pressing the play button. If it asks select the option `Attach using Process ID`. A dropdown should open, type `Touchdesigner` and select that process.

If the bottom bar turns orange it means that it was able to connect succesfully. Add a breakpoint in line 15 in vscode and trigger a Jump in the exampleExtension using the jump pulse. Vs code should stop at that line and allow you to inspect the state of the program in the left column.





# To do 

- [ ] Figure out how to use the `pathMappings` in `.vscode/launch.json` in order to not have to recreate the exact touchdesigner paths in Windows.