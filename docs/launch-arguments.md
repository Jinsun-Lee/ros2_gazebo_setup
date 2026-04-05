# launch 파일 실행 인자와 기본값

launch 파일의 모든 `DeclareLaunchArgument`는 기본값과 의미를 함께 문서화한다.

## 인자 선언
```python
from launch import LaunchDescription
from launch.actions import DeclareLaunchArgument
from launch.substitutions import LaunchConfiguration

def generate_launch_description():
    world_arg = DeclareLaunchArgument(
        'world',
        default_value='empty.sdf',
        description='로드할 SDF 월드 파일 경로'
    )

    use_sim_time_arg = DeclareLaunchArgument(
        'use_sim_time',
        default_value='true',
        description='Gazebo의 /clock을 ROS 시간으로 사용할지 여부'
    )

    return LaunchDescription([
        world_arg,
        use_sim_time_arg,
        # ... LaunchConfiguration('world')를 쓰는 노드들
    ])
```

<br>

## 프로젝트 전역 인자 규칙
| 인자            | 기본값        | 설명 |
|---------------|--------------|------|
| `world`       | `empty.sdf`  | 월드 파일명 또는 절대 경로 |
| `use_sim_time`| `true`       | ROS 시간을 Gazebo `/clock`과 동기화 |
| `robot_name`  | `robot`      | 모델 스폰 시 사용할 이름 |
| `x`, `y`, `z` | `0.0`        | 월드 프레임 기준 스폰 위치 |
| `yaw`         | `0.0`        | 스폰 yaw(라디안) |
| `rviz`        | `false`      | Gazebo와 함께 RViz 실행 여부 |
| `gui`         | `true`       | Gazebo GUI 실행 여부 (`false`는 headless) |
| `verbose`     | `false`      | Gazebo 상세 로깅 활성화 |

<br>

## CLI에서 인자 전달
```bash
ros2 launch <pkg> <launch_file> world:=my_world.sdf x:=1.0 rviz:=true
```

<br>

## 팁
- 프로젝트 내 launch 파일들은 인자명을 일관되게 유지한다.
- `use_sim_time`은 반드시 선언한다 — 누락 시 TF 타이밍 버그가 발생한다.
- 파일 경로는 절대 경로를 하드코딩하지 말고 `PathJoinSubstitution` + `FindPackageShare`를 사용한다.