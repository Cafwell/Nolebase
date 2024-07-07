
```python
import jieba
from gensim import corpora
from gensim.models import LdaModel
from gensim.parsing.preprocessing import STOPWORDS
import matplotlib.pyplot as plt
from wordcloud import WordCloud

# 自己定义
num_topics = 10
num_words = 50

# 读取文本数据
with open("test.txt", "r", encoding="utf-8") as f:
    corpus = f.read()

# 对文本数据进行分词
seg_list = jieba.cut(corpus)
words = [word for word in seg_list if len(word) > 1]


# 去除停用词
stopwords = set(STOPWORDS)
words = [word for word in words if word not in stopwords]

# 统计词频
word_freq = {}
for word in words:
    if word in word_freq:
        word_freq[word] += 1
    else:
        word_freq[word] = 1

# 将文本数据转换为gensim可用的格式
dictionary = corpora.Dictionary([words])
corpus = [dictionary.doc2bow([word]) for word in words]

# 使用LDA模型进行分析
lda_model = LdaModel(corpus, num_topics=10, id2word=dictionary)

# 输出所有主题及其对应的词语分布
#for i, topic in lda_model.print_topics(num_topics=-1):
#    print("Topic {}: {}".format(i, topic))

# 将每个主题的词按照权重组成一个字典
word_dict = {}
for i, topic in enumerate(lda_model.show_topics(num_topics=num_topics, num_words=num_words)):
    words = dict(lda_model.show_topic(i, topn=num_words))
    for word, weight in words.items():
        word_dict[word] = weight

# 创建词云对象，注意字体路径要自己设置，否则报错或乱码
wordcloud = WordCloud(font_path='C:/Windows/Fonts/msyh.ttc', background_color='white', width=800, height=600)

# 生成词云
wordcloud.generate_from_frequencies(word_dict)

# 显示词云
plt.figure(figsize=(10, 8))
plt.imshow(wordcloud)
plt.axis('off')
plt.show()
```


其中 test.txt 为分句（一行一句话最佳）。

附：[中文常用停用词表](https://github.com/goto456/stopwords)

