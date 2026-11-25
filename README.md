# 技能资格库的追溯更正与名额兑现

各记录目录保存独立的合成业务资料。SQL 文件仅包含原始关系表与已有行。manifest.json 记录库角色、表、行数及运行参数。timeline.json 记录后续客户端行为、来源提交或故障，用 pos 表示可比较的到达次序。同一 pos 按数组顺序。提交位置由相应记录的业务字段指定。

## 接入协议

people 以跨区统一参训人标识识别。facts 存机构原始声明，valid_from 含左端点，valid_to 排除右端点，NULL 为无右界。recorded 定义始终递增的登记位置。相同签发方用 fact_id 与 supersedes 关联修订；同一人、类型和业务日期若不同有效声明互相矛盾，登记时间不能决定谁可信。资格要有未撤回且互不矛盾的证书及与班次要求吻合的有效劳动关系。cert 与 relation 的值按班次所列工种、行业匹配。
seats 的 exclusive_from/exclusive_to 是班次兑现的排他期，两端采用左闭右开。holds 是演练开始前已经兑现的占用。held 可以释放，signed 已签约、paid 已出资。时间轴中的审核位置是已收到的事实版本坐标，review_day 指审核针对的业务日。

## 本地资料

使用 Python 3.11.2 和内置 SQLite 3.40.1。以下命令从记录创建关系库，目标文件需要尚不存在。各角色可以作为独立 SQLite 文件读取。

~~~sh
python3 seed_db.py fixtures/late-proof-and-two-offices main-local.db --role main
~~~

使用 Python 的 sqlite3 打开新文件即可访问原始表。用 python3 -m unittest -v 核对资料可装载、表与原始记录一致、SQLite 可同时打开读写连接。后续事件保存在各自的原始记录中。
