# Health

AIVA’s system health subsystem. It currently provides the `agi_logging` ROS 2
package, which persists ROS log events and broadcasts them for other AIVA
components. The repository is intended to grow to include error reporting,
runtime monitoring, and health checks.

## Current component

`agi_logging` subscribes to `/rosout`, writes structured JSON Lines records, and
publishes each record on `/agi/logs`. Records with severity `ERROR` or `FATAL`
go to `errors.jsonl`; other records go to `application.jsonl`. Both files use
rotation.

The log directory defaults to `~/.agi/log` and can be set with `AGI_LOG_DIR` or
the `log_dir` ROS parameter. Rotation is configured with the `max_bytes` and
`backup_count` ROS parameters.

## AIVA workspace integration

AIVA Shell launches this package as a separate ROS process. In the AGi workspace,
check out Health beside Shell and link the package into Shell’s colcon source
tree:

```sh
cd ~/AGi/AIVA
git clone https://github.com/Project-AGi/health.git health
ln -s ../../health shell/src/agi_logging
```

Then build and run it from the Shell workspace:

```sh
cd ~/AGi/AIVA/shell
source /opt/ros/jazzy/setup.bash
colcon build --packages-up-to agi_logging
source install/setup.bash
ros2 run agi_logging agi_log_collector
```

Shell remains responsible for starting and configuring the process. Health owns
the logging package and its event format.

## License

AGPL-3.0-only.
