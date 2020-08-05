
## SP 500 下載點
- https://finance.yahoo.com/quote/%5EGSPC/history/   


# 本機端使用standfordcore 的openie服務

要先下載 stanford-corenlp-4.0.0
並且使用  java -mx4g -cp "*" edu.stanford.nlp.pipeline.StanfordCoreNLPServer -port 9000 -timeout 50000
打開伺服器

```
from pycorenlp import *
import pandas as pd
from IPython import display
csv_f  = pd.read_csv("C://Users/88698/Desktop/CNN_post.csv",encoding = "ISO-8859-1")
nlp=StanfordCoreNLP("http://localhost:9000/")

relation = []
subject = []
object = []
time = []
#不能有str之外的資料
count = 0
for s in csv_f['tittle'] :
	output = nlp.annotate(s, properties={"annotators":"tokenize,ssplit,pos,depparse,natlog,openie","outputFormat": "json","triple.strict":"True",'openie.max_entailments_per_clause':'1000'})
	for i in range (len(output['sentences'])):
		result = [output["sentences"][i]["openie"] for item in output]
		for g in result:
			for rel in reversed(g):
				time.append(csv_f['time'][count])
				#relationSent=rel['subject'],rel['relation'],rel['object']
				print(rel['subject'],rel['relation'],rel['object'])
				relation.append(rel['relation'])
				subject.append(rel['subject'])
				object1.append(rel['object'])
				break
	count += 1


d = {'time':time,
	'sub' : subject,
        'relation' : relation,
        'obj' : object}
df = pd.DataFrame(data=d)
df.to_csv("C://Users/88698/Desktop/openie_data.csv")

```
