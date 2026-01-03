## MiFitness_hlth_center_aggregated_fitness_data.csv（按天聚合顶层）

➡️记录每天的聚合信息，相当于一个小日报

| 字段           | 含义                                                         |
| -------------- | ------------------------------------------------------------ |
| **Uid**        | 用户 ID                                                      |
| **Tag**        | 一级标签，有dsaily_report/mark/fitness                       |
| **Key**        | 具体的数据类型，如步数（steps）心跳（heart_rate)             |
| **Time**       | Unix 时间戳（秒）                                            |
| **Value**      | JSON 结构体，每个类型的数据都不同，具体可参考MiFitness_hlth_center_fitness_data.csv |
| **UpdateTime** | 记录更新时间                                                 |



| tag           | 含义                                     |
| ------------- | ---------------------------------------- |
| daily_fitness | **每日指标（步数，卡路里，高强度活动）** |
| daily_mark    | **每日特殊事件标记**                     |
| daily_report  | **每日身体指标**                         |

## 1.tag = daily_mark

```json
{    
    "has_data":true 	//value中的数据全是这种，统一不要了
}  
```



## 2.tag = daily_fitness

### key=goal（time字段，时间颗粒度，天）

```json
   
{"goal_items": [
        {
            "achieved_value": 81,		//今日消耗
            "target_value": 400,		//目标消耗
            "field": 2   				//卡路里
        },
        {
            "achieved_value": 1631,		//今日步数
            "target_value": 6000,		//目标步数
            "field": 1					//步数
        },
        {
            "achieved_value": 0,		//执行次数
            "target_value": 30,			//目标次数
            "field": 4					//中高强度活动次数
        }
    ],
//   sidList字段可能没有
     "sidList": [
        "hlth.gen_54960876160736",
        "huami.648502086500"
    ],
    "date_time": 1765238400,
    "timeInZero": 1765238400
}	
```

## 3.tag = daily_report

### key = valid_stand（time字段，时间颗粒度，天）

```json
{
    "count":3	//站立次数
}  
```

### key = steps

```json
{
    "calories": 46,		//卡路里
    "distance": 1021,	//距离，米
    "steps": 1631		//步数
}
```

### key = sleep

```json
{
    "avg_hr": 0,                      // 当天睡眠期间的平均心率。值为0可能表示数据缺失或未记录。
    "avg_spo2": 0,                    // 当天睡眠期间的平均血氧饱和度。值为0同样可能表示数据缺失。
    "day_sleep_evaluation": 0,        // 可能是对日间小睡（午睡）的评价代码。
    "segment_details": [              // 核心字段：一个包含了所有睡眠片段的列表。
        {
            "bedtime": 1765195560,      // 入睡时间 (秒级Unix时间戳)
            "sleep_deep_duration": 12,  // 深睡时长 (分钟)
            "duration": 74,             // 该片段睡眠总时长 (分钟)
            "sleep_light_duration": 62, // 浅睡时长 (分钟)
            "sleep_rem_duration": 0,    // 片段1的REM（快速眼动）睡眠时长 (分钟)
            "timezone": 32,             // 时区标识符
            "awake_count": 0,           // 中途清醒的次数
            "sleep_awake_duration": 0,  // 清醒的总时长 (分钟)
            "wake_up_time": 1765200000  // 睡醒时间 (秒级Unix时间戳)
        },
        {
            "bedtime": 1765128180,       
            "sleep_deep_duration": 101,  
            "duration": 496,           
            "sleep_light_duration": 329, 
            "sleep_rem_duration": 66,   
            "timezone": 32,            
            "awake_count": 1,          
            "sleep_awake_duration": 9,   
            "wake_up_time": 1765158480  
        }
    ],
    "long_sleep_evaluation": 6,       // 可能是对主要长睡眠（夜间睡眠）的评价代码。
    "sleep_awake_duration": 9,        // 当天所有睡眠片段中，清醒状态的总时长 (分钟)。(0 + 9 = 9)
    "sleep_deep_duration": 113,       // 当天所有睡眠片段中，深睡的总时长 (分钟)。(12 + 101 = 113)
    "sleep_light_duration": 391,      // 当天所有睡眠片段中，浅睡的总时长 (分钟)。(62 + 329 = 391)
    "sleep_rem_duration": 66,         // 当天所有睡眠片段中，REM睡眠的总时长 (分钟)。(0 + 66 = 66)
    "sleep_score": 78,                // 当天睡眠的综合总得分 (0-100)。
    "sleep_stage": 3,                 // 可能是对睡眠规律性或模式的评级代码。
    "total_duration": 570             // 当天所有睡眠片段的总时长 (分钟)。(74 + 496 = 570)
}
```

### key = heart_rate

```json
sid = default
{
    "abnormal_hr_count": 0,             // 当天检测到“异常心率”事件的次数 (例如心率过高或过低警报)。
    "aerobic_hr_zone_duration": 0,      // 当天心率处于“有氧耐力”区间的总时长 (单位可能是分钟)。
    "anaerobic_hr_zone_duration": 0,    // 当天心率处于“无氧极限”区间的总时长 (单位可能是分钟)。
    "avg_hr": 72,                       // 当天24小时的平均心率 (BPM, 次/分钟)。
    //    可能会没有avg_rhr这个字段
    "avg_rhr": 66,                      // 当天的平均静息心率 (Resting Heart Rate, BPM)。
    "extreme_hr_zone_duration": 0,      // 当天心率处于“极限燃脂”或“VO2 Max”区间的总时长 (单位可能是分钟)。
    "fat_burning_hr_zone_duration": 0,  // 当天心率处于“脂肪燃烧”区间的总时长 (单位可能是分钟)。
    "latest_hr": {                      // 记录了当天最后一次心率测量值的详细信息。
        "bpm": 91,                      // 最后一次测量到的心率值 (Beats Per Minute)。
        "time": 1765237320,             // 最后一次测量发生的时间 (秒级Unix时间戳)。
        "dbKey": "heart_rate",          // 数据库内部使用的键名，指明这是心率数据。
        "dbTime": 1765208520,           // 一个相关的数据库时间戳，可能是记录写入或关联的时间。
        "sid": "huami.648502086500"     // 来源或会话ID (Source/Session ID)，huami表明数据来自华米设备。
    },
    "max_hr": 112,                      // 当天记录到的最高心率 (BPM)。
    "min_hr": 54,                       // 当天记录到的最低心率 (BPM)。
    "warm_up_hr_zone_duration": 8       // 当天心率处于“热身放松”区间的总时长 (单位可能是分钟)。
}

还有一种形式
{
    "abnormal_hr_count": 0,				// 当天检测到“异常心率”事件的次数 (例如心率过高或过低警报)。
    "avg_hr": 64,						// 当天24小时的平均心率 (BPM, 次/分钟)。

    "avg_rhr": 64,						// 当天的平均静息心率 (Resting Heart Rate, BPM)。
    "max_hr": 104,						// 当天记录到的最高心率 (BPM)。
    "min_hr": 47						// 当天记录到的最低心率 (BPM)。
}


sid = xiaomisports_app
{
    "abnormal_hr_count": 0,				// 当天检测到“异常心率”事件的次数 (例如心率过高或过低警报)。
    "avg_hr": 64,						// 当天24小时的平均心率 (BPM, 次/分钟)。
    "avg_rhr": 64,						// 当天的平均静息心率 (Resting Heart Rate, BPM)。
    "max_hr": 104,						// 当天记录到的最高心率 (BPM)。
    "min_hr": 47						// 当天记录到的最低心率 (BPM)。
}

sid = huami*
{
    "avg_hr":68,
    "max_hr":110,
    "min_hr":55,
    "avg_rhr":null,
    "abnormal_hr_count":0
}
```

### key = calories

```json
{
    "calories": 79		//卡路里
}
```

### key = stress

```json
{
    "avg_stress": 22,               // 周期内的平均压力值 (0-100的评分)
    "max_stress": 81,               // 周期内记录到的最高压力值
    "min_stress": 3,                // 周期内记录到的最低压力值
    "stress_scale": {               // 压力等级分布统计
        "mild": 11,                 // 处于“轻度”压力状态的时长(分钟)
        "moderate": 11,             // 处于“中度”压力状态的时长(分钟)
        "relax": 83,                // 处于“放松”状态的时长(分钟)
        "severe": 1                 // 处于“严重”压力状态的时长(分钟)
    }
}
```



### key = intensity

```json
{
    "duration": 22,               // 运动强度持续时间 (分钟)
}
```

