# rqt_controller_bug

This project demonstrates a bug in rqt_controller_manager.

## Bug description

rqt_controller_manager seems to have a problem with controllers that switch from *Unloaded* to *Initialized/Stopped*.
Specifically, you are not able to right-click on controllers after the switch and their color doesn't switch from grey to red (even though the *state* column switches to *Initialized*).

## How to reproduce

```console
roslaunch rqt_controller_bug start_sim.launch
```

You should see three controllers in the gui, with different states. ```joint_position_controller``` is *Initialized*, but shown in grey (with right-click disabled).
You can still start the controller manually by running

```console
rosservice call /controller_manager/switch_controller "start_controllers: ['joint_position_controller']
  stop_controllers: ['']
  strictness: 1
  start_asap: false
  timeout: 0.0"
```

After this, the controller should switch to green and you can control it through the gui as usual.
Interestingly, switching the controller back by pressing *stop* works correctly (you can still start and unload it by mouse), but unloading it and loading it again brings back the bug.
