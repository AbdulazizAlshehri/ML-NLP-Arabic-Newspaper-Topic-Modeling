# <center>NLP: Topic Modeling for Riyadh Newspaper Articles</center>



**DATA**: We are planning to use our previously **scraped** data from the Riyadh Newspaper website **in Arabic**, ~3GB, plus some additional data (MetaData) that could add a meaningful sense to our project, see [Riyadh Newspaper website](http://www.alriyadh.com/1814297) to understand how we scraped the data.

**EXPECTED OUTPUT**: At the end of this project we expect to be done NLP **Topic Modeling** on Riyadh Newspaper articles, and all it's required preproccesing steps in addition to approperiate visualization.

**BONUS TASKS**: 
- Scraping ~3GB Data. 
- Using SQL Database for data storage. 
- As a challenge, we used data written in Arabic.


```python
import pandas  as pd
import numpy as np
import pyodbc
import nltk
```

### reading from Database


```python
# server = 't5.database.windows.net'
# database = 'T5'
# username = 't5'
# password = 'My404Data'

# # dfs2.columns=dfs2.columns.str.strip()

# cnxn = pyodbc.connect('DRIVER={SQL Server};SERVER='+server+';DATABASE='+database+';UID='+username+';PWD='+ password)
# cursor = cnxn.cursor()
```


```python
# df= pd.read_sql("select * from articles",cnxn)
# df
# df.to_csv("articles.csv", encoding='utf-8-sig')
```


```python
# df2= pd.read_sql("select top 10000 * from texts",cnxn)
# df2
# df2.to_csv("texts.csv",encoding='utf-8-sig')
```

### reading CSVs files


```python
df_articles = pd.read_csv('articles.csv')
```


```python
df_texts = pd.read_csv('texts.csv')
```

### preveiw


```python
df_articles.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>A_ID</th>
      <th>A_Link</th>
      <th>A_LinkNum</th>
      <th>A_Timestr</th>
      <th>A_Cat</th>
      <th>A_Auth</th>
      <th>A_Title1</th>
      <th>A_Title2</th>
      <th>A_Paragraphs</th>
      <th>A_Words</th>
      <th>A_Characters</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>1</td>
      <td>http://www.alriyadh.com/1814297</td>
      <td>1814297</td>
      <td>&amp;nbsp; \n\t\t\t\t\t\t\n\t\t\t\t\t\t\t\t\t\t\t\...</td>
      <td>المحليات</td>
      <td>الرياض - "الرياض"</td>
      <td>إلى ما بعد اجتماع مجموعة أوبك بلس</td>
      <td>السعودية تؤجل الإعلان عن أسعار الخام لشهر مايو</td>
      <td>8</td>
      <td>339</td>
      <td>1962</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>2</td>
      <td>http://www.alriyadh.com/1814297</td>
      <td>1814297</td>
      <td>&amp;nbsp; \n\t\t\t\t\t\t\n\t\t\t\t\t\t\t\t\t\t\t\...</td>
      <td>المحليات</td>
      <td>الرياض - "الرياض"</td>
      <td>إلى ما بعد اجتماع مجموعة أوبك بلس</td>
      <td>السعودية تؤجل الإعلان عن أسعار الخام لشهر مايو</td>
      <td>8</td>
      <td>339</td>
      <td>1962</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>3</td>
      <td>http://www.alriyadh.com/1814296</td>
      <td>1814296</td>
      <td>&amp;nbsp; \n\t\t\t\t\t\t\n\t\t\t\t\t\t\t\t\t\t\t\...</td>
      <td>المحليات</td>
      <td>وادي الدواسر - سعود آل مسيب</td>
      <td>NaN</td>
      <td>تعليم وادي الدواسر يبدأ المرحلة الثانية من خطة...</td>
      <td>3</td>
      <td>183</td>
      <td>1162</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>4</td>
      <td>http://www.alriyadh.com/1814296</td>
      <td>1814296</td>
      <td>&amp;nbsp; \n\t\t\t\t\t\t\n\t\t\t\t\t\t\t\t\t\t\t\...</td>
      <td>المحليات</td>
      <td>وادي الدواسر - سعود آل مسيب</td>
      <td>NaN</td>
      <td>تعليم وادي الدواسر يبدأ المرحلة الثانية من خطة...</td>
      <td>3</td>
      <td>183</td>
      <td>1162</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>5</td>
      <td>http://www.alriyadh.com/1814295</td>
      <td>1814295</td>
      <td>&amp;nbsp; \n\t\t\t\t\t\t\n\t\t\t\t\t\t\t\t\t\t\t\...</td>
      <td>المحليات</td>
      <td>الرياض - محمد الحيدر</td>
      <td>من برنامج إعانة الباحثين عن عمل</td>
      <td>"هدف" يودع 446 مليون ريال في حسابات المستفيدين</td>
      <td>4</td>
      <td>190</td>
      <td>1148</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_texts.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>T_ID</th>
      <th>T_AID</th>
      <th>T_Text</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>87160</td>
      <td>174309</td>
      <td>\nعلى الرغم من التحذيرات الشديدة والمتكررة الت...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>87164</td>
      <td>174317</td>
      <td>\nأدى محافظ ضمد عبدالله خالد البراق عقب صلاة ع...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>87165</td>
      <td>174319</td>
      <td>\nرفع رئيس مجلس إدارة الهيئة العامة للولاية عل...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>87168</td>
      <td>174325</td>
      <td>\nوجه صاحب السمو الملكي الأمير د. فيصل بن مشعل...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>87169</td>
      <td>174327</td>
      <td>\nاستقبل صاحب السمو الملكي الأمير فيصل بن خالد...</td>
    </tr>
  </tbody>
</table>
</div>



# Article Example


```python
df_texts['T_Text'][0]
```




    '\nعلى الرغم من التحذيرات الشديدة والمتكررة التي يوجهها الدفاع المدني للمواطنين قبل وبعد هطول الأمطار لتوخي الحذر والابتعاد عن أماكن تجمعات السيول وبطون الأودية ورغم ما تتناقله وسائل التواصل الاجتماعي من مقاطع مؤلمة ومفجعة لأشخاص جرفتهم السيول بمركباتهم ولقوا حتفهم بأبشع الصور وأسوأء المناظر، إلاّ أنه مازال هناك الكثير من المراهقين ومن فاقدي البصيرة المتهورين تستهويهم المغامرات المهلكة داخل بطون الأودية والأماكن التي غمرتها مياه الأمطار، غير مبالين بخطورة تلك الأفعال وشناعة تلك التصرفات العبثية، التي ربما يفقدون حياتهم بسببها في أدنى لحظة.\nوفي كل موسم من مواسم الأمطار تتجدد ظاهرة التهور والمغامرات في السيول وتتجدد معها معاناة رجال الدفاع المدني الذين مهما كان لديهم من إمكانات لن يستطيعوا تغطية جزء مساحته مئات الكيلومترات من براري المملكة الشاسعة التي تتنوع فيها التضاريس المعقدة.\nماذا بعد\nوتداولت شبكات التواصل الاجتماعي مقاطع فيديوهات لبعض المتهورين وهم يقتحمون الأودية مع سبق الإصرار، غير مبالين بتحذيرات الدفاع المدني وغير مبالين بالأشخاص الموجودين بتلك المواقع الذين حاولوا ثنيهم عن رأيهم ومنعهم من المخاطرة بأرواحهم، وكانت النتائج الواضحة في المقاطع كارثية.\nأحد المقاطع الذي استفز الدفاع المدني قاموا بوضعه في حسابهم في تويتر تحت هاشتاق #ماذا_بعد وطالبوا من المواطنين المشاركة في التعليق على الفيديو من أجل المساهمة في التوعية، بعض المغردين انتقد بلطف وطالب باستحداث نظام العقوبات لردع المتهورين، والبعض منهم هاجم المغامرين في السيول وقالوا: إنهم لا يستحقون الاهتمام بهم، وطالبوا من الدفاع المدني ألا يهرع لنجدتهم.\nالمغرد عاصم الرميح قال: أخي المتهور إذا كان لك محبون فلا تفجعهم، وإذا لم يكن لك محبون فلا تشغل الناس بهمك، وتذكر قول الله تعالى: «ولا تلقوا بأيديكم إلى التهلكة».\nوعلق طلال العتيبي قائلاً: ما يفعله بعض الشباب من تحدي السيول بمركباتهم واستعراض قيادتهم المتهورة في الأودية وتجمعات الأمطار لا ينم إلاّ عن تدني مستوى الوعي لديهم ورعونة التفكير وسلبية التصرف ومخالفة المنطق.\nسلوك خطير\nوأكد د.زيد بن عبدالله بن دريس –باحث في علم الجريمة- على أن من يتجاوز تعليمات وتحذيرات الدفاع المدني فقد قام بسلوك إجرامي خطير يؤدي بالنهاية إلى التهلكة لنفسه وللآخرين، مضيفاً أنه يجب على الجهات المختصة وضع حد لهذا السلوك من أجل ردعه، حيث إن السلوك الإنساني لا يرتدع أحياناً إلاّ بعقوبات سريعة وفعالة تجمح من سريان هذا السلوك المنحرف أو الإجرامي، وهذا ما تم مشاهدته أثناء هطول الأمطار وارتفاع مجرى السيول إلى مستويات عالية جداً تفوق المركبة أو الفرد نفسه، مشيراً إلى أنه لا توجد حتى الآن أي عقوبة صريحة تجاه من يقوم بمثل تلك الأفعال، وبالتالي استمرار ما نشاهده من استهتار من قبل بعض الأشخاص بحياتهم وحياة الآخرين وتدمير الممتلكات الشخصية من أجل المغامرة في مواقع السيول، مؤكداً على أن إقرار الغرامات المالية والسجن يُعد مطلباً أساسياً لمثل هذه التصرفات.\nحيطة وحذر\nوقال الشيخ الدكتور سعد السبر -إمام وخطيب جامع الجارالله بالرياض-: إن الله سبحانه وتعالى خلق الخلق لعبادته وجعل أرواحهم أمانة بين أيديهم ليس لهم حق التصرف عليها بالاعتداء\xa0بالقتل أو التعذيب أو الاعتداء على العقل بالخمور والمسكرات، مؤكداً على أن الشريعة جاءت بحفظ الضروريات الخمس\xa0الدين والنفس والعقل\xa0والمال والعرض، ومنع الاعتداء عليها بأي طريقة كانت أو بأي سبب مهما كان إلاّ بالحق الشرعي\xa0بإقامة الحدود\xa0والقصاص\xa0في النفس بشرط أن يُقيمها الحاكم أو نائبه فقط، ذاكراً إننا نشاهد البعض عند نزول الأمطار يتوجه إلى الصحراء ويتتبع المطر\xa0ومواقع نزوله ويتجول في الطرقات مع ما تقوم به الأجهزة الأمنية من تنبيه وتحذير للجميع من عدم الخروج وقت الأمطار، والانتباه وأخذ الحيطة والحذر، مبيناً أننا نسمع أخباراً\xa0عن حوادث المطر إمّا الوفاة، أو الإصابة بإعاقات، أو غرق للسيارات،\xa0أو غرق للبعض، فهذه مخالفات واعتداء على النفس وتعريضها للخطر والقتل.\nوأضاف: قال الله تعالى: «وَلا تَقْتُلُوا أَنْفُسَكُمْ إِنَّ اللَّهَ كانَ بِكُمْ رَحِيماً»، ومع كل هذه المخالفات في هذه التصرفات ستدخل الأحزان والهموم على الأسرة والمجتمع، ناصحاً أنه يجب على الشاب\xa0الحذر واتباع التعليمات والمحافظة على نفسه\xa0وممتلكاته ووطنه وأهليه.\n\n\n\nالدفاع المدني يبذل جهوداً في إنقاذ الغارقين'


