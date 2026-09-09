# 同济 EnquiryOfCourses 选课数据与抓取脚本

同济 `1.tongji.edu.cn`「课程查询(EnquiryOfCourses)」页面的选课数据快照，以及可按页面筛选条件重新抓取的脚本（Node / Python 双版本）。

## 数据

| 文件 | 说明 |
| --- | --- |
| `data/courses-2026-2027-1-raw.json` | 2026-2027 学年第 1 学期（calendarId=122）全量教学班原始 JSON，**1854 条** |
| `data/courses-2026-2027-1.csv` | 同上转成的 90 列扁平表（UTF-8 BOM，Excel 可直接打开） |

主要字段：课程编码 / 课程名称 / 学分 / 课程性质 / 开课学院 / 校区 / 培养层次 / 学习形式 / 教师 / 教学班 / 容量与已选人数 / 上课时间地点 / 结构化排课 `timeTableList` / 考核方式 / 教学语言等。

> 说明：该快照来自登录账号角色（研究生课程）在该查询页可检索到的全部教学班；不含任何个人隐私字段（`studentId` 等均为空）。如需其它学期，改脚本 `FILTERS.calendarId` 即可（可填 `1995` 至今任意学期 id）。

## 脚本用法（Node / Python 语义一致）

```bash
# 1) 先填鉴权：登录 1.tongji.edu.cn → F12 → Console → sessionStorage.getItem('sessionid')
#    把返回值填入脚本顶部 X_TOKEN

# 2) 查看所有可选真实选项（学期 id / 开课学院 deptCode / 课程性质 / 校区 / 培养层次 / 学习形式）
node fetch_courses.js options        # 或 python fetch_courses.py options

# 3) 编辑脚本顶部 FILTERS（留空 = 不过滤），然后抓取 → JSON + CSV
node fetch_courses.js                # 或 python fetch_courses.py
```

`FILTERS` 与页面筛选栏一一对应：`calendarId`(学期)、`newCourseCode`(课程编码)、`teachClassCode`(教学班号)、`courseName`(课程名)、`teacherName`(教师)、`faculty`(开课学院 deptCode)、`nature`(课程性质 1公共课/2专业课/3必修环节)、`campu`(校区 1四平路/2沪北/3嘉定/4沪西/5其他)、`trainingLevel`(培养层次 4硕士/6博士)、`formLearning`(学习形式 1全日制/2非全日制/4全日制&非全日制)。

已实测筛选：`courseName=数学` → 7 条；`faculty=000014`(研究生院) → 12 条；`nature=1&trainingLevel=4&campu=1` → 131 条。

## 鉴权与隐私

- 接口鉴权用请求头 `X-Token`（值为浏览器 `sessionStorage` 里的 `sessionid`），**不是 Cookie**。
- 脚本只含占位 token；请勿提交真实 token / Cookie / 抓包 HAR。数据文件已确认不含个人字段。

## 声明

本项目仅供学习与数据分析交流，禁止商用。使用请遵守同济大学相关规定与网站服务条款。
