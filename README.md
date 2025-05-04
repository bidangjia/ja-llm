# JAXX-LLM 数据分析项目

## 项目概述

本项目是一个基于Python的数据分析系统，主要用于处理和分析VOC(Voice of Customer)数据和社交媒体数据。项目集成了多个分析模块，包括产品场景匹配、用户需求提取、缺陷分析、社交媒体分析等功能，并提供了数据可视化和报告生成功能。

## 目录结构

```
├── Data/                          # 数据和提示词文件
│   ├── VOC相关提示词文件
│   ├── 建议表相关提示词文件
│   └── 原始数据文件
├── 生成结果/                      # 分析结果输出目录
│   ├── defect_analysis/           # 缺陷分析结果
│   ├── integrated_analysis_*/     # 各类综合分析结果
│   ├── matches_analysis/          # 场景匹配分析结果
│   ├── needs_analysis/            # 需求分析结果
│   ├── product_keywords/          # 产品关键词分析
│   ├── report_pdf/                # PDF报告输出
│   └── social_media/              # 社交媒体分析结果
└── Jupyter Notebooks              # 分析流程notebooks
    ├── 1.* 数据预处理和提取
    ├── 2.* VOC分析和可视化
    ├── 3.* 社交媒体分析
    ├── 4.* 结果汇总和报告生成
    └── 5.* 品牌分析
```

## 主要功能模块

### 1. VOC分析模块
- 产品场景匹配提取
- 用户需求分析
- 产品缺陷分析
- 品牌分析

### 2. 数据可视化模块
- 四象限分析图表
- 产品场景可视化
- 缺陷分布图
- 需求分布图

### 3. 社交媒体分析模块
- 数据预处理
- 理论假设生成
- 关键词提取
- 数据建模和回归分析

### 4. 报告生成模块
- 表格数据提取和生成
- PDF报告生成
- 分析结果汇总

## 使用指南

### 环境要求
- Python 3.10+
- Jupyter Notebook/Lab

### 环境准备和命令执行

1. 环境配置
```bash
# 创建工作目录
mkdir -p jaxx-llm && cd jaxx-llm

# 创建并激活虚拟环境
python3 -m venv venv
source venv/bin/activate  # macOS/Linux

# 安装依赖包
pip install jupyter pandas numpy matplotlib seaborn scikit-learn torch transformers
pip install openai langchain chromadb python-dotenv

# 配置环境变量
echo 'export OPENAI_API_KEY="your-api-key"' >> .env
echo 'export PYTHONPATH="${PYTHONPATH}:${PWD}"' >> .env
source .env

# 创建必要的目录
mkdir -p Data/VOC相关提示词文件 Data/建议表相关提示词文件 Data/原始数据文件
mkdir -p 生成结果/{defect_analysis,matches_analysis,needs_analysis,product_keywords,report_pdf,social_media,integrated_analysis_*}

# 确认数据文件准备
ls -l Data/原始数据文件/致欧-2025-01-10之后VOC数据.xlsx || echo "请将VOC数据文件放入Data/原始数据文件/目录"
ls -l Data/twitter_keywords_zo\ tech.{jsonl,xlsx} || echo "请将Twitter数据文件放入Data/目录"
```

2. 数据预处理模块
```bash
# 启动Jupyter Notebook并指定端口(默认8888)
jupyter notebook --port=8888 --notebook-dir=.

# 检查数据文件状态
python3 -c "import pandas as pd; df=pd.read_excel('Data/原始数据文件/致欧-2025-01-10之后VOC数据.xlsx'); print(f'数据行数: {len(df)}')" || echo "数据文件读取失败"

# 按顺序执行以下notebooks(确保每个步骤完成后检查输出):

# 1.0 数据预处理
jupyter nbconvert --to notebook --execute "1.0 数据预处理.ipynb" --output "1.0 数据预处理-执行结果.ipynb"
# 检查预处理输出
ls -l 生成结果/product_keywords/ || echo "预处理输出目录为空"

# 1.1 VOC-场景产品匹配提取
jupyter nbconvert --to notebook --execute "1.1 VOC-场景产品匹配提取.ipynb" --output "1.1 VOC-场景产品匹配提取-执行结果.ipynb"
# 检查匹配结果
ls -l 生成结果/matches_analysis/ || echo "场景匹配结果目录为空"

# 1.2 VOC-负向缺点读取
jupyter nbconvert --to notebook --execute "1.2 VOC-负向缺点读取.ipynb" --output "1.2 VOC-负向缺点读取-执行结果.ipynb"
# 检查缺陷分析结果
ls -l 生成结果/defect_analysis/ || echo "缺陷分析结果目录为空"

# 1.3 VOC-产品需求提取
jupyter nbconvert --to notebook --execute "1.3 VOC-产品需求提取.ipynb" --output "1.3 VOC-产品需求提取-执行结果.ipynb"
# 检查需求分析结果
ls -l 生成结果/needs_analysis/ || echo "需求分析结果目录为空"
```

3. VOC分析模块（可并行执行）
```bash
# 设置matplotlib后端为Agg以支持无界面环境
export MPLBACKEND="Agg"

# 产品场景分析
jupyter nbconvert --to notebook --execute "2.1 VOC-输出图像-产品场景画图.ipynb" \
  --ExecutePreprocessor.timeout=600 --output "2.1 VOC-输出图像-产品场景画图-执行结果.ipynb"
# 检查场景分析图表输出
ls -l 生成结果/matches_analysis/*场景*.png || echo "场景分析图表生成失败"

# 缺陷分析
jupyter nbconvert --to notebook --execute "2.2 VOC-输出图像-缺陷画图.ipynb" \
  --ExecutePreprocessor.timeout=600 --output "2.2 VOC-输出图像-缺陷画图-执行结果.ipynb"
# 检查缺陷分析图表输出
ls -l 生成结果/defect_analysis/*缺陷*.png || echo "缺陷分析图表生成失败"

# 需求分析
jupyter nbconvert --to notebook --execute "2.3 VOC-输出图像-需求画图.ipynb" \
  --ExecutePreprocessor.timeout=600 --output "2.3 VOC-输出图像-需求画图-执行结果.ipynb"
# 检查需求分析图表输出
ls -l 生成结果/needs_analysis/*需求*.png || echo "需求分析图表生成失败"

# 表格生成
jupyter nbconvert --to notebook --execute "2.4 VOC-表格提取.ipynb" \
  --ExecutePreprocessor.timeout=600 --output "2.4 VOC-表格提取-执行结果.ipynb"
# 检查表格提取结果
ls -l 生成结果/*/*.xlsx || echo "表格提取失败"

jupyter nbconvert --to notebook --execute "2.5 VOC-表格书写.ipynb" \
  --ExecutePreprocessor.timeout=600 --output "2.5 VOC-表格书写-执行结果.ipynb"
# 检查表格生成结果
ls -l 生成结果/*/*.txt || echo "表格生成失败"
```

4. 社交媒体分析模块
```bash
# 检查社交媒体数据文件
python3 -c "import pandas as pd; df=pd.read_json('Data/twitter_keywords_zo tech.jsonl', lines=True); print(f'Twitter数据行数: {len(df)}')" || echo "Twitter数据文件读取失败"

# 设置社交媒体分析参数
export TWITTER_API_KEY="your-twitter-api-key"
export TWITTER_API_SECRET="your-twitter-api-secret"

# 按顺序执行以下notebooks:

# 数据预处理
jupyter nbconvert --to notebook --execute "3.0 社媒-数据预处理.ipynb" \
  --ExecutePreprocessor.timeout=600 --output "3.0 社媒-数据预处理-执行结果.ipynb"
# 检查预处理输出
ls -l 生成结果/social_media/preprocessed_data.csv || echo "社媒数据预处理失败"

# 理论假设生成
jupyter nbconvert --to notebook --execute "3.1 社媒-理论假设生成.ipynb" \
  --ExecutePreprocessor.timeout=600 --output "3.1 社媒-理论假设生成-执行结果.ipynb"
# 检查假设生成结果
ls -l 生成结果/social_media/hypotheses.txt || echo "理论假设生成失败"

# 关键词生成
jupyter nbconvert --to notebook --execute "3.2 社媒-关键词生成.ipynb" \
  --ExecutePreprocessor.timeout=600 --output "3.2 社媒-关键词生成-执行结果.ipynb"
# 检查关键词结果
ls -l 生成结果/social_media/keywords.json || echo "关键词生成失败"

# 数据筛选
jupyter nbconvert --to notebook --execute "3.3 社媒-数据筛选.ipynb" \
  --ExecutePreprocessor.timeout=600 --output "3.3 社媒-数据筛选-执行结果.ipynb"
# 检查筛选结果
ls -l 生成结果/social_media/filtered_data.csv || echo "数据筛选失败"

# 数据赋值
jupyter nbconvert --to notebook --execute "3.4 社媒-数据赋值.ipynb" \
  --ExecutePreprocessor.timeout=600 --output "3.4 社媒-数据赋值-执行结果.ipynb"
# 检查赋值结果
ls -l 生成结果/social_media/labeled_data.csv || echo "数据赋值失败"

# 数据建模
jupyter nbconvert --to notebook --execute "3.5 社媒-数据建模.ipynb" \
  --ExecutePreprocessor.timeout=600 --output "3.5 社媒-数据建模-执行结果.ipynb"
# 检查模型输出
ls -l 生成结果/social_media/model_results.json || echo "数据建模失败"

# 数据可视化
jupyter nbconvert --to notebook --execute "3.6 社媒-数据可视化.ipynb" \
  --ExecutePreprocessor.timeout=600 --output "3.6 社媒-数据可视化-执行结果.ipynb"
# 检查可视化输出
ls -l 生成结果/social_media/*.png || echo "数据可视化失败"

# 回归结果提取
jupyter nbconvert --to notebook --execute "3.7 社媒-回归结果提取.ipynb" \
  --ExecutePreprocessor.timeout=600 --output "3.7 社媒-回归结果提取-执行结果.ipynb"
# 检查回归分析结果
ls -l 生成结果/social_media/regression_results.{csv,json} || echo "回归结果提取失败"
```

5. 结果汇总模块
```bash
# 检查所有必要的分析结果是否已生成
python3 -c '
import os
required_dirs = ["defect_analysis", "matches_analysis", "needs_analysis", "social_media"]
for d in required_dirs:
    if not os.path.exists(f"生成结果/{d}") or not os.listdir(f"生成结果/{d}"):
        print(f"警告: {d}分析结果不完整")
'

# 四象限图生成
# 产品场景四象限图
jupyter nbconvert --to notebook --execute "4.1 汇总四象限图-产品场景.ipynb" \
  --ExecutePreprocessor.timeout=600 --output "4.1 汇总四象限图-产品场景-执行结果.ipynb"
# 检查产品场景四象限图输出
ls -l 生成结果/last_quadrant/*产品场景*.png || echo "产品场景四象限图生成失败"

# 痛点四象限图
jupyter nbconvert --to notebook --execute "4.2 汇总四象限图-痛点.ipynb" \
  --ExecutePreprocessor.timeout=600 --output "4.2 汇总四象限图-痛点-执行结果.ipynb"
# 检查痛点四象限图输出
ls -l 生成结果/last_quadrant/*痛点*.png || echo "痛点四象限图生成失败"

# 需求四象限图
jupyter nbconvert --to notebook --execute "4.3 汇总四象限图-需求.ipynb" \
  --ExecutePreprocessor.timeout=600 --output "4.3 汇总四象限图-需求-执行结果.ipynb"
# 检查需求四象限图输出
ls -l 生成结果/last_quadrant/*需求*.png || echo "需求四象限图生成失败"

# 表格汇总
# 直接读取汇总
jupyter nbconvert --to notebook --execute "4.4 表格写作汇总-直接读取.ipynb" \
  --ExecutePreprocessor.timeout=600 --output "4.4 表格写作汇总-直接读取-执行结果.ipynb"
# 检查直接读取结果
ls -l 生成结果/integrated_analysis_*/*.xlsx || echo "表格直接读取失败"

# RAG方式汇总
jupyter nbconvert --to notebook --execute "4.5 表格写作汇总-RAG.ipynb" \
  --ExecutePreprocessor.timeout=600 --output "4.5 表格写作汇总-RAG-执行结果.ipynb"
# 检查RAG汇总结果
ls -l 生成结果/integrated_analysis_*/*RAG*.{xlsx,json} || echo "RAG表格汇总失败"

# 生成最终报告
jupyter nbconvert --to notebook --execute "4.6 总结PDF生成-六图.ipynb" \
  --ExecutePreprocessor.timeout=600 --output "4.6 总结PDF生成-六图-执行结果.ipynb"
# 检查PDF报告输出
ls -l 生成结果/report_pdf/*.pdf || echo "PDF报告生成失败"
```

6. 品牌分析模块（可选）
```bash
# 检查品牌分析所需数据
python3 -c '
import pandas as pd
df = pd.read_excel("Data/原始数据文件/致欧-2025-01-10之后VOC数据.xlsx")
if "品牌" not in df.columns:
    print("警告: 数据中缺少品牌信息列")
else:
    print(f"发现{df["品牌"].nunique()}个不同品牌")
'

# 品牌维度分析
# 产品场景分析
jupyter nbconvert --to notebook --execute "5.1 VOC-品牌分析-产品场景.ipynb" \
  --ExecutePreprocessor.timeout=600 --output "5.1 VOC-品牌分析-产品场景-执行结果.ipynb"
# 检查品牌场景分析结果
ls -l 生成结果/integrated_analysis_matches/brand_*.{png,xlsx} || echo "品牌场景分析失败"

# 缺陷分析
jupyter nbconvert --to notebook --execute "5.1 VOC-品牌分析-缺陷.ipynb" \
  --ExecutePreprocessor.timeout=600 --output "5.1 VOC-品牌分析-缺陷-执行结果.ipynb"
# 检查品牌缺陷分析结果
ls -l 生成结果/integrated_analysis_defect/brand_*.{png,xlsx} || echo "品牌缺陷分析失败"

# 需求分析
jupyter nbconvert --to notebook --execute "5.1 VOC-品牌分析-需求.ipynb" \
  --ExecutePreprocessor.timeout=600 --output "5.1 VOC-品牌分析-需求-执行结果.ipynb"
# 检查品牌需求分析结果
ls -l 生成结果/integrated_analysis_needs/brand_*.{png,xlsx} || echo "品牌需求分析失败"

# 原声提取
jupyter nbconvert --to notebook --execute "5.2 VOC-对应原声.ipynb" \
  --ExecutePreprocessor.timeout=600 --output "5.2 VOC-对应原声-执行结果.ipynb"
# 检查原声提取结果
ls -l 生成结果/*/*.txt | grep -i "原声" || echo "原声提取失败"
```

### 分析流程

1. 数据预处理（必须按顺序执行）
   1. `1.0 数据预处理.ipynb` - 初始数据清洗和准备
   2. `1.1 VOC-场景产品匹配提取.ipynb` - 提取产品场景匹配信息
   3. `1.2 VOC-负向缺点读取.ipynb` - 提取产品缺陷信息
   4. `1.3 VOC-产品需求提取.ipynb` - 提取用户需求信息

2. VOC分析（可并行执行，但依赖于数据预处理完成）
   1. 产品场景分析：`2.1 VOC-输出图像-产品场景画图.ipynb`
   2. 缺陷分析：`2.2 VOC-输出图像-缺陷画图.ipynb`
   3. 需求分析：`2.3 VOC-输出图像-需求画图.ipynb`
   4. 表格生成：
      - `2.4 VOC-表格提取.ipynb`
      - `2.5 VOC-表格书写.ipynb`

3. 社交媒体分析（必须按顺序执行，可与VOC分析并行）
   1. `3.0 社媒-数据预处理.ipynb` - 社交媒体数据清洗
   2. `3.1 社媒-理论假设生成.ipynb` - 生成分析假设
   3. `3.2 社媒-关键词生成.ipynb` - 提取关键词
   4. `3.3 社媒-数据筛选.ipynb` - 数据筛选和过滤
   5. `3.4 社媒-数据赋值.ipynb` - 数据特征工程
   6. `3.5 社媒-数据建模.ipynb` - 建立分析模型
   7. `3.6 社媒-数据可视化.ipynb` - 生成可视化图表
   8. `3.7 社媒-回归结果提取.ipynb` - 提取分析结果

4. 结果汇总（依赖于VOC分析和社交媒体分析完成）
   1. 四象限图生成：
      - `4.1 汇总四象限图-产品场景.ipynb`
      - `4.2 汇总四象限图-痛点.ipynb`
      - `4.3 汇总四象限图-需求.ipynb`
   2. 表格汇总：
      - `4.4 表格写作汇总-直接读取.ipynb`
      - `4.5 表格写作汇总-RAG.ipynb`
   3. `4.6 总结PDF生成-六图.ipynb` - 生成最终报告

5. 品牌分析（可选，依赖于VOC分析完成）
   - 品牌维度分析：
     - `5.1 VOC-品牌分析-产品场景.ipynb`
     - `5.1 VOC-品牌分析-缺陷.ipynb`
     - `5.1 VOC-品牌分析-需求.ipynb`
   - `5.2 VOC-对应原声.ipynb` - 提取原始评论

## 输出结果

分析结果将保存在 `生成结果/` 目录下，包括：
- 四象限分析图表
- 产品场景匹配结果
- 用户需求分析报告
- 缺陷分析报告
- 社交媒体分析结果
- PDF格式的综合分析报告

## 注意事项

- 运行notebooks时请确保按照编号顺序执行
- 确保Data目录中包含所需的原始数据文件
- 分析结果会自动保存到相应的输出目录中