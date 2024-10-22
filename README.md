#### Reddit数据采集
##### 注：执行环境需能够访问外网

##### Step 1: submission_ids_crawler.py
- 功能：根据submission id获取对应帖子信息，解析入库
- 运行设置：需指定subreddit、start_time和end_time参数；可指定node(1~20)并行执行。
- 生成文件：submission_ids/{}_subreddit_id.txt

##### Step 2: submission_crawler.py
- 功能：获取Reddit某板块所有submission id
- 运行前置条件：利用 sql/*.sql 文件为各板块建表
- 运行设置：需指定subreddit、start_idx和end_idx参数；可指定node(1~20)并行执行。
- 运行结果：存入数据库表 submission_{} 

##### Step 3: export_db_to_csv.py
- 功能：将数据库中的数据导出为csv文件
- 运行设置：需指定subreddit、start_idx和end_idx参数；可指定node(1~20)并行执行。
- 生成文件：submissions/submission_{}.csv
---
#### Reddit数据预处理
##### 运行前置条件：将采集的数据从数据库导出，放在 submissions/submission_{}.csv

##### Step 1：extract_from_submissions.py
1. extract_posts()
- 功能：从采集的帖子数据中提取所有评论及子评论
- 运行设置：指定待处理的 subreddits 列表
- 生成文件：submission_allposts/submission_{}_allposts.csv

2. select_sarcasm()
- 功能：根据讽刺符"/s"将讽刺评论过滤出来
- 运行设置：指定待处理的 subreddits 列表
- 生成文件：sarcasm_data/sarcasm_{}.csv

##### Step 2：create_context.py
- 功能：获取讽刺评论对应的父级和祖父级评论数据
- 运行设置：指定待处理的 subreddits 列表
- 生成文件：unselected_for_label/{}_unselected_for_label.xlsx

##### Step 3：create_dataset.py
- 功能：过滤讽刺评论，构建为标注数据集格式
- 运行设置：指定待处理的 subreddits 列表
- 生成文件：output_file/{}_pair.txt（即待标注数据集）