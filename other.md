## 这些表中数据主要为运动相关，可参考 [running_page](https://github.com/yihong0618/running_page) 这个项目





## MiFitness_hlth_center_data_source.csv

➡️ 设备表，记录了设备的id，创建时间，名称等  

| 字段           | 含义                                                         |
| -------------- | ------------------------------------------------------------ |
| **Uid**        | 用户 ID                                                      |
| **Sid**        | 设备ID                                                       |
| **Identifier** | 应该是设备的，名称？                                         |
| **Model**      | 处于的模块，对个人来说没用                                   |
| **Name**       | 名称，大多都是乱码                                           |
| **Alias**      | NULL                                                         |
| Status         | NULL                                                         |
| CreateTime     | 创建时间，有趣的是，他用的是**纳秒级 Unix 时间戳**，豆包解释说常用于分布式数据库高并发下的时间唯一性 |
| UpdateTime     | 更新时间                                                     |





## MiFitness_hlth_center_sport_record.csv（用的不多，忽略）

➡️ **每次运动记录（跑步、步行、骑行、力量等）**
 包含的内容：运动类型，开始/结束时间，时长，距离。热量。心率统计。GPS 有无。体育细分类别等

| 字段           | 含义                                                         |
| -------------- | ------------------------------------------------------------ |
| **Uid**        | 用户 ID                                                      |
| **Did**        | 设备 ID（手表、手环）                                        |
| **Key**        | 运动类型（type）例如： - `outdoor_run`（户外跑） - `indoor_run`（跑步机） - `walking` - `cycling` |
| **Time**       | Unix 时间戳（运动开始时间）                                  |
| **Value**      | JSON 结构体，包含大量运动详情                                |
| **UpdateTime** | 记录更新时间                                                 |

### Value中的JSON， sport_type（运动类型枚举）常见值：

| sport_type | 含义         |
| ---------- | ------------ |
| 1          | 户外跑       |
| 2          | 室内跑       |
| 3          | 户外走       |
| 4          | 室内走       |
| 5          | 户外骑行     |
| 6          | 室内骑行     |
| 7          | 游泳         |
| 11         | 力量训练     |
| 18         | 椭圆机       |
| 100+       | App 扩展运动 |





##  **MiFitness_hlth_center_sport_track_data.csv**

➡️ **运动轨迹数据（GPX 下载链接）**
 每条运动记录对应一个 GPX URL，字段包括：

- Did（设备 ID）
- Key（运动模式，例如 outdoor_run_class）
- Time（运动开始时间）
- GPX（包含轨迹的下载地址）

| 字段           | 说明                                 |
| -------------- | ------------------------------------ |
| **Uid**        | 用户 ID                              |
| **Did**        | 设备 ID                              |
| **Key**        | 运动类型（通常是 outdoor_run_class） |
| **Time**       | 运动开始时间（timestamp）            |
| **UpdateTime** | 更新时间                             |
| **GPX**        | GPX 文件的下载 URL（用来导出轨迹）   |



## MiFitness_user_device_setting.csv

用户设备设置数据



## MiFitness_user_fitness_profile.csv

空



## MiFitness_user_fitness_with_uuid_data_records.csv

空



## MiFitness_user_health_plan_records.csv

空



## MiFitness_user_member_profile.csv

空



## MiFitness_user_pk_invite_record.csv

空



## MiFitness_user_pk_record.csv

空



## MiFitness_user_pk_result_statistics.csv

空



## MiWear_hlth_user_course_train_record.csv

空



## MiWear_hlth_user_course_train_statistics.csv

空



