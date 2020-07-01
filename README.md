# 工作日常

## 2020-07-01 :sunflower:
1. 七月第一天，过去一个多月github坚持天天更新，已经养成习惯了。七月继续加油，每天进步一点点。
2. 身体很重要，继续坚持锻炼。
3. python层次聚类
   1. 方法一：使用scipy库,linkage()输入为稀疏矩阵或稠密矩阵，输出为(n-1)*4的矩阵，n为样本数量，共n-1次聚合，dendrogram()函数可视化聚类树状图。该方法在我的数据集上报错，因为数据深度超过限制。
      ```
      	import scipy
	from scipy.cluster.hierarchy import linkage,dendrogram
	import matplotlib.pyplot as plt
	Z = linkage(tfidf.toarray())
	dendrogram(Z)
	plt.show()
      ```

   2. 方法二：使用sklearn库，[参考资料](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.AgglomerativeClustering.html#sklearn.cluster.AgglomerativeClustering)
	    ```
	    from sklearn.cluster import AgglomerativeClustering
	    c = AgglomerativeClustering(n_clusters=6).fit_predict(tfidf.toarray()) 
	    #TypeError: A sparse matrix was passed, but dense data is required. Use X.toarray() to convert to a dense numpy array
	    ```
   3. 原始的职业数据 由用户填空而来，存在大量较短、频次较低的职业数据，仅依靠tf-idf计算可能不行，在计算tf-idf前需要过滤掉仅出现一次的词。
   4. 数据清洗未做好，聚类的结果也不理想。



## 2020-06-30 :stuck_out_tongue_closed_eyes:
1. jieba计算tf_idf，[参考资料](https://github.com/fxsjy/jieba#%E5%9F%BA%E4%BA%8E-tf-idf-%E7%AE%97%E6%B3%95%E7%9A%84%E5%85%B3%E9%94%AE%E8%AF%8D%E6%8A%BD%E5%8F%96)
- jieba.analyse.extract_tags(sentence, topK=20, withWeight=False, allowPOS=())
  - sentence 为待提取的文本
  - topK 为返回几个 TF/IDF 权重最大的关键词，默认值为 20
  - withWeight 为是否一并返回关键词权重值，默认值为 False
	- allowPOS 仅包括指定词性的词，默认值为空，即不筛选
- jieba.analyse.TFIDF(idf_path=None) 新建 TFIDF 实例，idf_path 为 IDF 频率文件
- 对于df['col']这种文本，即文本存储在Series里，这种方式不适合。

2. 制作词云:ok_hand:
- 使用jieba和wordcloud库
- 参考资料1：https://juejin.im/post/5b0f962751882515773af358  
- 参考资料2：https://amueller.github.io/word_cloud/generated/wordcloud.WordCloud.html#wordcloud.WordCloud

3. sklearn计算df['col']的tfidf
- sklearn.feature_extraction.text.CountVectorizer()
	- 功能：统计原始数据的词的频次
  - 代码：先实例化，再使用fit_transform(data)方法返回document—term matrix。data为可迭代对象，不能包含空值、非str
- sklearn.feature_exyraction.text.TfidfTransformer()
	- 功能：
	- 代码：先实例化，再使用fit_transform(X)方法。X可为fit_transform(data)的输出，`{array-like, sparse matrix, dataframe} of shape (n_samples, n_features)`
	- 输出：X_newndarray array of shape (n_samples, n_features_new)。与X类似，但值现在为tf-idf值，不再是{0,1}
- 多行数据样本，所有的词。计算出的tf-idf为n*m的矩阵。每行数据不出现的词的tf-idf为0。

4. 对于sklearn计算的tf-idf，如何层次聚类呢？层次聚类能否实现目标-将凌乱的填空数据统一化？

## 2020-06-29 :sunny::kissing:
1. 投放环节数据处理 
   1. jupyter notebook读取PG和MySQL数据，pandas处理，输出csv文件 :smile:
