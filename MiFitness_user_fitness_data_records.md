## MiFitness_user_fitness_data_records.csv（分钟聚合中层）

➡️它核心记录了您在一段时间内，由穿戴设备（如小米手环或手表）及关联设备（如体脂秤）产生的**各类健康与运动数据**。这不仅仅是简单的数字，而是包含了**每日总结**、**单次运动详情**、**睡眠分段采样**、**心率**、**体重**等多个维度、相互关联的详细信息

| 列名          | 字段含义                | 详细说明                                                     |
| ------------- | ----------------------- | ------------------------------------------------------------ |
| `uid`         | **用户ID (User ID)**    | 您的小米账户的唯一数字标识符，用于关联所有数据到您的个人账户。 |
| `did`         | **设备ID (Device ID)**  | 产生这条数据的设备的唯一标识符，可以区分数据是来自您的手表、手环还是其他设备。 |
| `tag`         | **数据一级分类**        | 用于区分数据的大类别。例如 `days` 代表按天聚合的记录，`sports` 代表单次运动记录，`times` 通常代表体重等在特定时间点测量的数据。 |
| `key`         | **数据二级分类**        | 在一级分类下，对数据类型的进一步细分。例如，当`tag`是`days`时，`key`可能是`watch_sleep_report`（睡眠总结）或`watch_night_sleep_record`（睡眠采样点）。 |
| `time`        | **记录生成时间**        | 这条记录被创建或最终确定的时间。这是一个**以秒为单位**的10位Unix时间戳。例如，一份日报的`time`通常是当天结束时的零点。 |
| `last_modify` | **最后修改时间**        | 这条记录最后一次被更新的时间，格式与`time`列相同（秒级Unix时间戳）。 |
| `metric`      | **达标标识**            | 根据说明文档，此列用于标记是否达到某个目标。它的值可能在不同数据类型下有不同含义，例如标记某天运动是否达标。 |
| `value`       | **核心数据 (JSON格式)** | 这是最重要的列，包含了具体详细的健康数据，其内容为JSON格式。它的结构**完全取决于**同一行的`tag`和`key`，可以是一次跑步的完整数据，也可以是一晚睡眠的详细报告。 |

### 📌表中tag列，值的含义

| `tag` 值            | 含义               | 详细说明                                                     |
| ------------------- | ------------------ | ------------------------------------------------------------ |
| `days`              | **每日聚合数据**   | 包含以“天”为单位的聚合或总结性数据，例如一整天的睡眠报告、心率区间统计等。 |
| `sports`            | **单次运动记录**   | 记录了每一次具体的运动，如一次完整的户外跑步、一次室内单车等。`value`中包含该次运动的所有详细指标。 |
| `times`             | **时间点测量数据** | 记录在某个精确时间点发生的数据，最典型的例子就是体重测量。   |
| `once`              | **单次手动测量**   | 用户主动、单次触发的测量行为，比如在非运动状态下手动测量一次心率。 |
| `weekly_statistics` | **周度统计报告**   | 包含以“周”为单位的统计报告，例如每周的健康总结。             |
| `medal_statistics`  | **成就与奖牌统计** | 记录您获得的各种运动成就或虚拟奖牌的数据。                   |
| `sports_category_*` | **运动分类归档**   | 这是一个动态的分类，`*`部分会变化（如`sports_category_run`），用于在App内部对不同类型的运动进行归档整理。 |
| `object_name`       | **关联数据路径**   | 这种`tag`下的`key`通常是一串字符，可能指向一个更详细的数据文件，例如GPS轨迹文件。 |



### 📌列表中key值对应的含义，一级tag对应的二级key有很多组合，这里只有我要用的，其他的都在下面

| Tag（数据一级分类） | Key（数据二级分类）      | 核心含义                                                     |
| ------------------- | ------------------------ | ------------------------------------------------------------ |
| days                | watch_steps_report       | 每日步数报告（汇总单日总步数、距离及步数目标达成情况）       |
| days                | watch_steps_record       | 每日步数记录（按时间段细分的单日步数、距离明细数据）         |
| days                | watch_sleep_report       | 每日睡眠报告（汇总单日夜间睡眠总时长、深浅睡时长及睡眠评分） |
| days                | watch_night_sleep_record | 夜间睡眠记录（按时间段细分的夜间睡眠状态、时长明细数据）     |
| days                | watch_hrm_report         | 每日心率报告（汇总单日平均心率、最高 / 最低心率及静息心率）  |
| days                | huami_pai_record         | 华米 PAI 记录（单日个人活动智能指数，含不同强度运动区间的 PAI 值） |
| days                | watch_stress_report      | 每日压力报告（汇总单日平均压力、最高 / 最低压力及压力状态分布） |
| weekly_statistics   | fitness_report           | 周健身报告（汇总每周步数、卡路里、睡眠、心率等综合健康数据） |
|                     |                          |                                                              |

## 二、各 Key 对应的 Value 列 JSON 字段含义

### 1. Key: watch_steps_report（每日步数报告）

```json
{
  "steps": 217,                      // 单日总步数
  "distance": 139,                   // 单日总距离（单位：米）
  "actSteps": {"0": 217},            // 实际步数（0为默认设备标识，值为217）
  "goal": 6000                       // 单日步数目标（6000步）
}
```

### 2. Key: watch_steps_record（每日步数记录）

```json
[
  {
    "time": 41400,                   // 时间段（秒，对应当日11:30）
    "steps": 42,                     // 该时间段步数
    "distance": 26,                  // 该时间段距离（单位：米）
    "actSteps": {"0": 42}            // 该时间段实际步数（设备标识0）
  },
  {
    "time": 43200,                   // 时间段（秒，对应当日12:00）
    "steps": 175,                    // 该时间段步数
    "distance": 113,                 // 该时间段距离（单位：米）
    "actSteps": {"0": 175}           // 该时间段实际步数（设备标识0）
  }
]
```

### 3. Key: watch_sleep_report（每日睡眠报告）

```json
{
  "bedtime": 1679159400,             // 入睡时间戳
  "wake_up_time": 1679194860,        // 醒来时间戳
  "night_duration": 537,             // 夜间睡眠总时长（单位：分钟）
  "sleep_deep_duration": 76,         // 深度睡眠时长（单位：分钟）
  "sleep_light_duration": 347,       // 浅度睡眠时长（单位：分钟）
  "sleep_rem_duration": 114,         // REM睡眠时长（单位：分钟）
  "sleep_awake_duration": 54,        // 夜间清醒时长（单位：分钟）
  "total_score": 67,                 // 睡眠总评分（0-100，67为良好）
  "friendly_score": 67               // 睡眠友好评分（与总评分一致）
}
```

### 4. Key: watch_night_sleep_record（夜间睡眠记录）

```json
{
  "items": [                         // 睡眠分段记录列表
    {
      "start": 1679159400,           // 该段睡眠开始时间戳
      "end": 1679160540,             // 该段睡眠结束时间戳
      "state": 3,                    // 睡眠状态（3=深睡）
      "seg_end": false               // 是否为睡眠分段结束标记（false=否）
    },
    // （其余分段结构相同，省略重复项）
    {
      "start": 1679193480,           // 最后一段睡眠开始时间戳
      "end": 1679194860,             // 最后一段睡眠结束时间戳（与醒来时间一致）
      "state": 3,                    // 睡眠状态（3=深睡）
      "seg_end": false               // 分段结束标记（false=否）
    }
  ],
  "duration": 537                    // 夜间睡眠总时长（单位：分钟）
}
```

| `state` 值 | 含义                   |
| ---------- | ---------------------- |
| 2          | **浅睡 (Light Sleep)** |
| 3          | **深睡 (Deep Sleep)**  |
| 4          | **REM (快速眼动)**     |
| 5          | **清醒 (Awake)**       |

2和3的定义可能颠倒了，后续统计数据的时候再看

### 5. Key: watch_hrm_report（每日心率报告）

```json
{
  "avg_hrm": 64,                     // 日均心率（单位：次/分钟）
  "max_hrm": {                       // 当日最高心率
    "time": 1679161920,              // 最高心率出现时间戳
    "hrm": 95                        // 最高心率值（单位：次/分钟）
  },
  "min_hrm": {                       // 当日最低心率
    "time": 1679197800,              // 最低心率出现时间戳
    "hrm": 47                        // 最低心率值（单位：次/分钟）
  },
  "rhr_avg": 64                      // 日均静息心率（单位：次/分钟）
}
```

### 6. Key: huami_pai_record（华米 PAI 记录）

```json
{
  "daily_pai": 0.006605863571166992, // 当日PAI值（个人活动智能指数）
  "total_pai": 1.5958819389343262,   // 累计PAI值
  "high_zone_pai": 0,                // 高强度运动区间PAI值
  "medium_zone_pai": 0,              // 中强度运动区间PAI值
  "low_zone_pai": 0.0066058636       // 低强度运动区间PAI值
}
```

### 7. Key: watch_stress_report（每日压力报告）

```json
{
  "avg_stress": 18,                  // 日均压力值（0-100，18为轻度压力）
  "max_stress": {                    // 当日最高压力
    "stress": 81,                    // 最高压力值
    "time": 1679116980               // 最高压力出现时间戳
  },
  "min_stress": {                    // 当日最低压力
    "stress": 1,                     // 最低压力值
    "time": 1679086440               // 最低压力出现时间戳
  },
  "stress_scale": {                  // 压力等级分布（单位：小时）
    "mild": 9,                       // 轻度压力时长
    "moderate": 3,                   // 中度压力时长
    "relax": 87,                     // 放松状态时长
    "severe": 1                      // 重度压力时长
  },
  "total_stress_duration": 100,      // 总压力状态时长（单位：小时）
  "last_time_offset": 83760,         // 最后一次压力记录时间偏移（单位：秒）
  "stress_proportion": {             // 压力状态
    "mild": 8,                       // 轻度压力分钟
    "moderate": 2,                   // 中度压力分钟
    "relax": 90,                     // 放松状态分钟
    "severe": 0                      // 重度压力分钟
  }
}
```

### 8. Key: fitness_report（周健身报告）

```json
{
  "sport_times": 0,                  // 本周运动次数
  "sport_times_diff": 0,             // 与上周运动次数的差值（0表示无变化）
  "sport_days": 0,                   // 本周运动天数
  "sport_days_diff": 0,              // 与上周运动天数的差值
  "steps_summary": {                 // 本周步数汇总
    "field_type": "int_value",       // 字段类型（整数型）
    "field_unit": "step",            // 单位（步）
    "int_value": 35052               // 本周总步数
  },
  "step_goal_achieve": 2,            // 本周步数目标达成次数
  "calorie_summary": {               // 本周卡路里汇总
    "field_type": "int_value",       // 字段类型（整数型）
    "field_unit": "kcal",            // 单位（千卡）
    "int_value": 1021                // 本周总卡路里消耗
  },
  "rank_percent_summary": {          // 本周健康数据排名百分比（相对其他用户）
    "1": 0.43,                       // 维度1（推测为步数）排名百分比（43%）
    "2": 0.19                        // 维度2（推测为卡路里）排名百分比（19%）
  },
  "all_fitness_goal_achieved_count": 0, // 本周所有健身目标均达成的次数
  "allFitnessGoalAchivevedDay": null,   // 所有目标均达成的日期（null表示无）
  "all_fitness_goal_achieved_count_diff": 0, // 与上周所有目标达成次数的差值
  "avg_achieved_fitness_goal_summary": { // 每日平均达成目标汇总
    "1764518400": {                  // 日期对应的时间戳（本周起始日期）
      "1": {                         // 目标类型1（步数）
        "field_type": "int_value",
        "field_unit": "step",
        "int_value": 5007            // 日均步数目标达成值
      },
      "2": {                         // 目标类型2（卡路里）
        "field_type": "int_value",
        "field_unit": "kcal",
        "int_value": 145             // 日均卡路里目标达成值
      },
      "3": {                         // 目标类型3（运动次数）
        "field_type": "int_value",
        "field_unit": "1",           // 单位（次）
        "int_value": 2               // 日均运动次数目标达成值
      },
      "4": {                         // 目标类型4（运动时长）
        "field_type": "int_value",
        "field_unit": "min",         // 单位（分钟）
        "int_value": 1               // 日均运动时长目标达成值
      }
    }
  },
  "sleep_report": {                  // 本周睡眠报告
    "avg_sleep_score": 51,           // 日均睡眠评分（0-100，越高睡眠质量越好）
    "max_sleep_score": 67,           // 本周最高睡眠评分
    "avg_sleep_duration": 709,       // 日均睡眠总时长（单位：分钟）
    "avg_deep_sleep_duration": 86,   // 日均深度睡眠时长（单位：分钟）
    "avg_deep_sleep_rate": 13,       // 日均深度睡眠占比（13%）
    "sleep_stage": 1,                // 睡眠阶段等级（1为基础等级）
    "sleep_evaluation": 9,           // 睡眠综合评价等级（9为较好等级）
    "week_lastest_bed_time": 1764895260, // 本周最晚入睡时间（时间戳）
    "week_lastest_bed_time_zone": 8, // 最晚入睡时间对应的时区（+8时区）
    "week_earliest_wake_up_time": 1764969596, // 本周最早醒来时间（时间戳）
    "week_earliest_wake_up_zone": 8  // 最早醒来时间对应的时区（+8时区）
  },
  "hlth_status": {                   // 本周健康状态汇总
    "avg_rhr": 65,                   // 日均静息心率（单位：次/分钟）
    "max_hr": 119,                   // 本周最高心率（单位：次/分钟）
    "avg_stress": 18,                // 日均压力值（0-100，越低压力越小）
    "max_stress": 87                 // 本周最高压力值
  },
  "version": 1                       // 报告数据版本号（V1）
}
```





###  以下为运动相关数据，用的不多，不重点查看

| Tag（数据一级分类）      | Key（数据二级分类）                     | 核心含义                                                     |
| ------------------------ | --------------------------------------- | ------------------------------------------------------------ |
| weekly_statistics        | update_hlthcenter_records               | 健康中心记录更新（同步健康数据至中心数据库）                 |
| weekly_statistics        | start_calculate                         | 周数据计算启动（触发每周运动健康数据统计计算）               |
| weekly_statistics        | update_records                          | 历史记录更新（同步过往健康数据记录）                         |
| times                    | watch_weight_record                     | 手表体重记录（通过手表设备录入的单次体重数据）               |
| sports_category_walking  | watch_sport_outdoor_walking             | 户外步行运动分类标记（归类户外步行相关运动数据）             |
| sports_category_training | watch_sport_free_training               | 自由训练运动分类标记（归类无固定类型的自由训练数据）         |
| sports_category_running  | watch_sport_outdoor_running             | 户外跑步运动分类标记（归类户外跑步相关运动数据）             |
| sports_category_running  | watch_sport_indoor_running              | 室内跑步运动分类标记（归类室内跑步相关运动数据）             |
| sports_category_cycling  | watch_sport_outdoor_riding              | 户外骑行运动分类标记（归类户外骑行相关运动数据）             |
| sports                   | watch_sport_outdoor_running             | 户外跑步运动数据（记录单次户外跑步的时长、距离、心率等详细数据） |
| sports                   | watch_sport_outdoor_walking             | 户外步行运动数据（记录单次户外步行的时长、距离、心率等详细数据） |
| sports                   | watch_sport_indoor_running              | 室内跑步运动数据（记录单次室内跑步的时长、距离、心率等详细数据） |
| sports                   | watch_sport_outdoor_riding              | 户外骑行运动数据（记录单次户外骑行的时长、距离、心率等详细数据） |
| sports                   | watch_sport_free_training               | 自由训练运动数据（记录单次自由训练的时长、卡路里、心率等详细数据） |
| reach_goal               | daily_goal_calorie                      | 每日卡路里达标记录（跟踪每日卡路里消耗目标的达成情况及连续达标天数） |
| once                     | huami_manual_heartrate_record           | 华米手动心率记录（手动录入的单次心率测量数据）               |
| once                     | watch_single_stress_record              | 手表单次压力记录（手表设备测量的单次身体压力数据）           |
| medal_statistics         | medal_stats_month_steps                 | 月度步数勋章统计（统计月度每日步数及步数目标达成情况，关联勋章获取） |
| medal_statistics         | medal_stats_month_calorie               | 月度卡路里勋章统计（统计月度每日卡路里消耗及相关目标达成情况） |
| medal_statistics         | sport_days_watch_sport_outdoor_running  | 户外跑步运动天数统计（统计特定周期内户外跑步的有效运动天数） |
| medal_statistics         | sport_days_total_sports                 | 总运动天数统计（统计特定周期内所有类型运动的有效运动天数）   |
| medal_statistics         | sport_days_sports_medal_running         | 跑步勋章运动天数统计（统计与跑步勋章相关的有效运动天数）     |
| medal_statistics         | sport_days_watch_sport_outdoor_walking  | 户外步行运动天数统计（统计特定周期内户外步行的有效运动天数） |
| medal_statistics         | sport_days_watch_sport_outdoor_riding   | 户外骑行运动天数统计（统计特定周期内户外骑行的有效运动天数） |
| medal_statistics         | sport_days_watch_sport_indoor_running   | 室内跑步运动天数统计（统计特定周期内室内跑步的有效运动天数） |
| medal_statistics         | sport_days_watch_sport_free_training    | 自由训练运动天数统计（统计特定周期内自由训练的有效运动天数） |
| medal_statistics         | sports_medal_running                    | 跑步勋章数据统计（汇总跑步相关勋章获取的总里程、总步数、总卡路里等数据） |
| medal_statistics         | sport_stats_watch_sport_outdoor_walking | 户外步行运动统计（汇总所有户外步行的累计数据，如总距离、总卡路里等） |
| medal_statistics         | sport_stats_watch_sport_outdoor_running | 户外跑步运动统计（汇总所有户外跑步的累计数据，如总距离、总卡路里等） |
| medal_statistics         | sport_stats_watch_sport_outdoor_riding  | 户外骑行运动统计（汇总所有户外骑行的累计数据，如总距离、总卡路里等） |
| medal_statistics         | sport_stats_watch_sport_indoor_running  | 室内跑步运动统计（汇总所有室内跑步的累计数据，如总距离、总卡路里等） |
| medal_statistics         | sport_stats_watch_sport_free_training   | 自由训练运动统计（汇总所有自由训练的累计数据，如总时长、总卡路里等） |
| medal_statistics         | sport_stats_total_data                  | 总运动数据统计（汇总所有运动类型的累计数据，含总步数、总距离等） |
| medal_statistics         | medal_stats_total_steps                 | 总步数勋章统计（汇总历史总步数及步数目标达成总次数，关联勋章获取） |
| medal_statistics         | medal_stats_total_calorie               | 总卡路里勋章统计（汇总历史总卡路里消耗及目标达成总次数，关联勋章获取） |
| days                     | watch_regular_target_report             | 每日常规目标报告（汇总单日步数、卡路里、运动时长等常规目标的达成情况） |
| days                     | watch_regular_target_record             | 每日常规目标记录（按时间段细分的单日步数、卡路里达标明细数据） |
| days                     | watch_calories_report                   | 每日卡路里报告（汇总单日总卡路里消耗及卡路里目标达成情况）   |
| days                     | watch_calories_record                   | 每日卡路里记录（按时间段细分的单日卡路里消耗明细数据）       |
| days                     | watch_manual_spo2_report                | 手动血氧报告（汇总单日手动测量的血氧相关数据）               |
|                          |                                         |                                                              |
| days                     | watch_hrm_record                        | 每日心率记录（单日原始心率数据，含连续测量的心率信息）       |
|                          |                                         |                                                              |
| days                     | huami_calories_record                   | 华米卡路里记录（按时间段细分的华米设备记录的单日卡路里消耗明细） |
|                          |                                         |                                                              |
| days                     | watch_stress_record                     | 每日压力记录（单日原始压力数据，含连续测量的压力信息）       |
| days                     | watch_daytime_sleep_record              | 日间睡眠记录（按时间段细分的日间小憩状态、时长明细数据）     |
| days                     | huami_regular_active_stage              | 华米常规活动阶段记录（细分单日不同活动阶段的运动数据，如步行、跑步等） |
| days                     | watch_mh_intensity_record               | 手表运动强度记录（记录单日运动强度相关数据）                 |

### 1. Key: watch_sport_outdoor_running（户外跑步运动数据）

```json
{
  "timestamp": 1649767358,           // 数据生成时间戳
  "timezone": 32,                    // 时区编码（对应+8时区）
  "version": 1,                      // 数据版本号
  "start_time": 1649767358,          // 运动开始时间戳
  "end_time": 1649767777,            // 运动结束时间戳
  "duration": 417,                   // 运动总时长（单位：秒）
  "distance": 1284,                  // 运动总距离（单位：米）
  "calories": 87,                    // 运动消耗卡路里（单位：千卡）
  "max_pace": 203,                   // 最大配速（单位：秒/公里）
  "min_pace": 2992,                  // 最小配速（单位：秒/公里）
  "avg_pace": 324,                   // 平均配速（单位：秒/公里）
  "max_speed": 17.726385,            // 最大速度（单位：公里/小时）
  "steps": 1160,                     // 运动总步数
  "max_cadence": 0,                  // 最大步频（单位：步/分钟，0表示无数据）
  "avg_cadence": 167,                // 平均步频（单位：步/分钟）
  "avg_hrm": 160,                    // 平均心率（单位：次/分钟）
  "max_hrm": 185,                    // 最大心率（单位：次/分钟）
  "min_hrm": 85,                     // 最小心率（单位：次/分钟）
  "rise_height": 0,                  // 累计上升高度（单位：米，0表示无海拔变化）
  "fall_height": 0,                  // 累计下降高度（单位：米，0表示无海拔变化）
  "avg_height": -20000,              // 平均海拔高度（单位：米，-20000表示无有效数据）
  "max_height": -20000,              // 最高海拔高度（单位：米，无有效数据）
  "min_height": -20000               // 最低海拔高度（单位：米，无有效数据）
}
```

### 2. Key: watch_sport_outdoor_walking（户外步行运动数据）

```json
{
  "timestamp": 1637624878,           // 数据生成时间戳
  "timezone": 32,                    // 时区编码（对应+8时区）
  "version": 1,                      // 数据版本号
  "start_time": 1637624878,          // 运动开始时间戳
  "end_time": 1637625130,            // 运动结束时间戳
  "duration": 251,                   // 运动总时长（单位：秒）
  "distance": 401,                   // 运动总距离（单位：米）
  "calories": 30,                    // 运动消耗卡路里（单位：千卡）
  "max_pace": 532,                   // 最大配速（单位：秒/公里）
  "min_pace": 2298,                  // 最小配速（单位：秒/公里）
  "avg_pace": 625,                   // 平均配速（单位：秒/公里）
  "max_speed": 6.755566,             // 最大速度（单位：公里/小时）
  "steps": 487,                      // 运动总步数
  "max_cadence": 0,                  // 最大步频（0表示无数据）
  "avg_cadence": 116,                // 平均步频（单位：步/分钟）
  "avg_hrm": 119,                    // 平均心率（单位：次/分钟）
  "max_hrm": 131,                    // 最大心率（单位：次/分钟）
  "min_hrm": 111,                    // 最小心率（单位：次/分钟）
  "rise_height": 0,                  // 累计上升高度（0表示无海拔变化）
  "fall_height": 0,                  // 累计下降高度（0表示无海拔变化）
  "avg_height": -20000,              // 平均海拔高度（无有效数据）
  "max_height": -20000,              // 最高海拔高度（无有效数据）
  "min_height": -20000               // 最低海拔高度（无有效数据）
}
```

### 3. Key: watch_sport_indoor_running（室内跑步运动数据）

```json
{
  "timestamp": 1625634103,           // 数据生成时间戳
  "timezone": 32,                    // 时区编码（对应+8时区）
  "version": 1,                      // 数据版本号
  "start_time": 1625634103,          // 运动开始时间戳
  "end_time": 1625638588,            // 运动结束时间戳
  "duration": 4484,                  // 运动总时长（单位：秒）
  "distance": 70,                    // 运动总距离（单位：米）
  "calories": 335,                   // 运动消耗卡路里（单位：千卡）
  "max_pace": 850,                   // 最大配速（单位：秒/公里）
  "min_pace": 9981,                  // 最小配速（单位：秒/公里）
  "avg_pace": 63503,                 // 平均配速（单位：秒/公里）
  "steps": 180,                      // 运动总步数
  "max_cadence": 0,                  // 最大步频（0表示无数据）
  "avg_cadence": 2,                  // 平均步频（单位：步/分钟）
  "avg_hrm": 96,                     // 平均心率（单位：次/分钟）
  "max_hrm": 144,                    // 最大心率（单位：次/分钟）
  "min_hrm": 69                      // 最小心率（单位：次/分钟）
}
```

### 4. Key: watch_sport_outdoor_riding（户外骑行运动数据）

```json
{
  "timestamp": 1623564192,           // 数据生成时间戳
  "timezone": 32,                    // 时区编码（对应+8时区）
  "version": 1,                      // 数据版本号
  "start_time": 1623564192,          // 运动开始时间戳
  "end_time": 1623565709,            // 运动结束时间戳
  "duration": 1516,                  // 运动总时长（单位：秒）
  "distance": 5953,                  // 运动总距离（单位：米）
  "calories": 232,                   // 运动消耗卡路里（单位：千卡）
  "max_pace": 166,                   // 最大配速（单位：秒/公里）
  "min_pace": 1629,                  // 最小配速（单位：秒/公里）
  "avg_pace": 254,                   // 平均配速（单位：秒/公里）
  "max_speed": 21.607155,            // 最大速度（单位：公里/小时）
  "avg_hrm": 135,                    // 平均心率（单位：次/分钟）
  "max_hrm": 155,                    // 最大心率（单位：次/分钟）
  "min_hrm": 105,                    // 最小心率（单位：次/分钟）
  "rise_height": 0,                  // 累计上升高度（0表示无海拔变化）
  "fall_height": 0,                  // 累计下降高度（0表示无海拔变化）
  "avg_height": -20000,              // 平均海拔高度（无有效数据）
  "max_height": -20000,              // 最高海拔高度（无有效数据）
  "min_height": -20000,              // 最低海拔高度（无有效数据）
  "total_climbing": 0                // 累计爬升高度（0表示无爬升）
}
```

### 5. Key: watch_sport_free_training（自由训练运动数据）

```json
{
  "timestamp": 1603026305,           // 数据生成时间戳
  "timezone": 32,                    // 时区编码（对应+8时区）
  "version": 1,                      // 数据版本号
  "start_time": 1603026305,          // 运动开始时间戳
  "end_time": 1603027264,            // 运动结束时间戳
  "duration": 958,                   // 运动总时长（单位：秒）
  "calories": 154,                   // 运动消耗卡路里（单位：千卡）
  "avg_hrm": 133,                    // 平均心率（单位：次/分钟）
  "max_hrm": 165,                    // 最大心率（单位：次/分钟）
  "min_hrm": 99                      // 最小心率（单位：次/分钟）
}
```

### 6. Key: daily_goal_calorie（每日卡路里达标记录）

```json
{
  "items": [                         // 每日达标记录列表
    {"time": 1664121600, "count": 1}, // 日期时间戳+当日达标次数（1次）
    {"time": 1664208000, "count": 2}, // 日期时间戳+当日达标次数（2次）
    // （其余记录结构相同，省略重复项）
    null                             // 无达标记录的日期（null表示无数据）
  ],
  "item_count": 35,                  // 有效达标记录总数（35条）
  "first_index": 13,                 // 第一条有效记录的索引位置
  "last_index": 3,                   // 最后一条有效记录的索引位置
  "continuous_days": {               // 连续达标天数统计
    "time": 1665158400,              // 连续达标结束日期时间戳
    "count": 13                      // 最长连续达标天数（13天）
  },
  "max_continuous_days": 13          // 历史最长连续达标天数（13天）
}
```

### 7. Key: huami_manual_heartrate_record（华米手动心率记录）

记录不连续，不考虑用这个字段

```json
{
  "hrm": 93                          // 手动测量的心率值（单位：次/分钟）
}
```

### 8. Key: watch_single_stress_record（手表单次压力记录）

小米导出的数据表中，这个值并不是连续的，在有这条记录的一年中，该字段只出现了140次

```json
{
  "stress": 38                       // 单次测量的压力值（0-100，38为轻度压力）
}
```

### 10. Key: medal_stats_month_steps（月度步数勋章统计）

```json
{
  "steps_lst": [3475, 2944, ..., 0], // 月度每日步数列表（按日期顺序排列，0表示无数据）
  "goals_lst": [0, 0, ..., 0]        // 月度每日步数目标达成标记（1=达成，0=未达成）
}
```

### 11. Key: medal_stats_month_calorie（月度卡路里勋章统计）

```json
{
  "calorie_lst": [79, 63, ..., 0],   // 月度每日卡路里消耗列表（按日期顺序，0表示无数据）
  "goals_lst": null                  // 卡路里目标达成标记（null表示未设置目标）
}
```

### 12. Key: sports_medal_running（跑步勋章数据统计）

```json
{
  "total_achievement": {             // 跑步总成就数据
    "avg_pace": {                    // 平均配速
      "field_type": "int_value",
      "field_unit": "sec",           // 单位（秒/公里）
      "int_value": 286               // 历史平均配速
    },
    "calories": {                    // 总卡路里消耗
      "field_type": "int_value",
      "field_unit": "kcal",
      "int_value": 28129             // 跑步总卡路里消耗
    },
    "distances": {                   // 总跑步距离
      "field_type": "int_value",
      "field_unit": "metre",         // 单位（米）
      "int_value": 303816            // 跑步总距离
    },
    "steps": {                       // 总跑步步数
      "field_type": "int_value",
      "field_unit": "step",
      "int_value": 330171            // 跑步总步数
    }
  },
  "total_duration": 152670,          // 跑步总时长（单位：秒）
  "total_calorie": 28129,            // 跑步总卡路里消耗（单位：千卡）
  "total_times": 80,                 // 跑步总次数
  "total_days": 80,                  // 跑步总天数
  "real_time_scan_time": 0,          // 实时扫描时间（0表示无实时扫描）
  "history_scan_end_time": 0         // 历史扫描结束时间（0表示无历史扫描记录）
}
```

### 20. Key: huami_regular_active_stage（华米常规活动阶段记录）

```json
{
  "stages": [                        // 活动阶段列表
    {
      "start": 523,                  // 阶段开始时间（分钟，当日08:43）
      "stop": 538,                   // 阶段结束时间（分钟，当日08:58）
      "mode": 7,                     // 活动模式（7=步行）
      "dis": 101,                    // 该阶段距离（单位：米）
      "cal": 4,                      // 该阶段卡路里消耗（单位：千卡）
      "nCal": 0,                     // 该阶段净卡路里消耗（0表示无数据）
      "step": 158                    // 该阶段步数
    },
    // （其余阶段结构相同，省略重复项）
    {
      "start": 1264,                 // 阶段开始时间（分钟，当日21:04）
      "stop": 1270,                  // 阶段结束时间（分钟，当日21:10）
      "mode": 1,                     // 活动模式（1=跑步）
      "dis": 293,                    // 该阶段距离（单位：米）
      "cal": 9,                      // 该阶段卡路里消耗（单位：千卡）
      "nCal": 0,                     // 该阶段净卡路里消耗
      "step": 453                    // 该阶段步数
    }
  ]
}
```

## 补充说明

1. **时间戳与时间段**：所有`timestamp`/`start_time`/`end_time`为 Unix 秒级时间戳，可通过在线工具转换为常规日期时间；`time`字段若为小数（如 41400），表示当日累计秒数（41400 秒 = 11 小时 30 分钟）。
2. **睡眠状态编码**：结合行业标准推测，`2=深睡`、`3=浅睡`、`4=REM睡眠`、`5=清醒`。
3. **活动模式编码**：`1=跑步`、`3=骑行`、`7=步行`，其余编码待补充。
4. **数据单位说明**：步数单位为 “步”，距离为 “米”，时长为 “秒”（运动数据）或 “分钟”（睡眠数据），心率为 “次 / 分钟”，卡路里为 “千卡”。
5. **设备标识**：`did`字段中`default_did`为默认设备，`xiaomisports_app`为小米运动 APP，`huami.xxx`为华米设备，对应数据来源的设备类型。

