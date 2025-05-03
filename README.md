# UK2007 Spam Detection Project

## 数据集获取与准备

本项目使用 [WEBSPAM-UK2007](https://chato.cl/webspam/datasets/uk2007/features/) 数据集的三个特征集：

1. **Feature set 1: direct features**  
   下载链接：[uk-2007-05.obvious_features.csv.gz](https://chato.cl/webspam/datasets/uk2007/features/uk-2007-05.obvious_features.csv.gz)  
   解压后重命名为：`1.uk-2007-05.obvious_features.csv`

2. **Feature set 2b: transformed link-based features**  
   下载链接：[uk-2007-05.link_based_features_transformed.csv.gz](https://chato.cl/webspam/datasets/uk2007/features/uk-2007-05.link_based_features_transformed.csv.gz)  
   解压后重命名为：`2.uk-2007-05.link_based_features_transformed.csv`

3. **Feature set 3a: content-based features**  
   下载链接：[uk-2007-05.content_based_features.csv.gz](https://chato.cl/webspam/datasets/uk2007/features/uk-2007-05.content_based_features.csv.gz)  
   解压后重命名为：`3.uk-2007-05.content_based_features.csv`

请确保解压后的文件名与上述一致，并放在项目根目录下，如下图所示：

```
1.uk-2007-05.obvious_features.csv
2.uk-2007-05.link_based_features_transformed.csv
3.uk-2007-05.content_based_features.csv
```

## RMarkdown 文件

可以一个一个 block 运行，来熟悉项目，也可以一次生成报告见下

## RMarkdown 分析报告

本项目的分析主文件为 `output.rmd`，可通过 R 或 RStudio 渲染为 HTML 报告。

### 渲染方法

#### 在 R/RStudio 控制台运行

```r
rmarkdown::render("output.rmd")
```

这会在当前目录下生成 `output.html`。

#### 在终端（shell）下运行

```sh
Rscript -e "rmarkdown::render('output.rmd')"
```

同样会生成 `output.html`。

### 依赖包

请确保已安装以下 R 包（如未安装可用 `install.packages("包名")` 安装）：

- tidyverse
- skimr
- corrplot
- scales
- kableExtra
- caret
- pROC
- randomForest
- e1071

### 其他说明

- 生成的 `output.html` 会与 `output.rmd` 位于同一目录。
- 如遇到包缺失、报错或需要批量渲染、指定输出路径等问题，欢迎随时提问！

---

**数据集及特征集详细介绍请参考：[WEBSPAM-UK2007 官方页面](https://chato.cl/webspam/datasets/uk2007/features/)**
