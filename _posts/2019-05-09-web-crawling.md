---
layout:     post
title:      "Basic Web Crawling & Regular Expression"
subtitle:   " "
date:       2019-05-09 16:11:00 +0900
background: '/img/universe.jpg'
comments:   true
categories: python
---

Python의 BeautifulSoup을 이용하면 아주 간단하게 웹 크롤링을 할 수 있다. 이번 포스트에서는 Html 소스를 가져와서 정규표현식을 이용해 간단하게 전처리하는 부분까지 다루었다. 웹 크롤링을 하려면 html에 대한 기본 지식은 가지고 있는 것이 좋다.
<br><br>
```python
from bs4 import BeautifulSoup
import requests
import re
```
requests는 HTTP 요청을 보내는 모듈이고, re는 정규표현식 모듈이다. 위키피디아에서 인어공주 삽입곡의 트랙 리스트를 가져와보자.
<br><br>
```python
html = requests.get ("https://en.wikipedia.org/wiki/The_Little_Mermaid_(soundtrack)").text
soup = BeautifulSoup(html, 'html5lib')
trackListRaw = soup.find('table', {'class': 'tracklist'})
```
페이지 소스를 확인하고 가져오려고 하는 문단, 표 등의 태그를 넣는다. 이때 class 또는 id가 있으면 특정 부분만 사용하기 좋다. trackListRaw를 확인하면,

```python
trackListRaw
```
```profile
    <table class="tracklist" style="display:block;border-spacing:0px;border-collapse:collapse;padding:4px"><tbody><tr><th class="tlheader" scope="col" style="width:2em;padding-left:10px;padding-right:10px;text-align:right;background-color:#eee"><abbr title="Number">No.</abbr></th><th class="tlheader" scope="col" style="width:60%;text-align:left;background-color:#eee">Title</th><th class="tlheader" scope="col" style="width:40%;text-align:left;background-color:#eee">Recording artist(s)</th><th class="tlheader" scope="col" style="width:4em;padding-right:10px;text-align:right;background-color:#eee">Length</th></tr><tr style="background-color:#fff"><td style="padding-right:10px;text-align:right;vertical-align:top">1.</td><td style="vertical-align:top">"Fathoms Below"</td><td style="vertical-align:top">Ship's Chorus</td><td style="padding-right:10px;text-align:right;vertical-align:top">1:41</td></tr><tr style="background-color:#f7f7f7"><td style="padding-right:10px;text-align:right;vertical-align:top">2.</td><td style="vertical-align:top">"Main Titles" <span style="font-size:85%">(Score)</span></td><td style="vertical-align:top"> </td><td style="padding-right:10px;text-align:right;vertical-align:top">1:26</td></tr><tr style="background-color:#fff"><td style="padding-right:10px;text-align:right;vertical-align:top">3.</td><td style="vertical-align:top">"<a href="/wiki/Fanfare" title="Fanfare">Fanfare</a>" <span style="font-size:85%">(Score)</span></td><td style="vertical-align:top"> </td><td style="padding-right:10px;text-align:right;vertical-align:top">0:27</td></tr><tr style="background-color:#f7f7f7"><td style="padding-right:10px;text-align:right;vertical-align:top">4.</td><td style="vertical-align:top">"Daughters of Triton"</td><td style="vertical-align:top"><a href="/wiki/Kimmy_Robertson" title="Kimmy Robertson">Kimmy Robertson</a>, <a href="/wiki/Caroline_Vasicek" title="Caroline Vasicek">Caroline Vasicek</a></td><td style="padding-right:10px;text-align:right;vertical-align:top">0:38</td></tr><tr style="background-color:#fff"><td style="padding-right:10px;text-align:right;vertical-align:top">5.</td><td style="vertical-align:top">"<a href="/wiki/Part_of_Your_World" title="Part of Your World">Part of Your World</a>"</td><td style="vertical-align:top"><a href="/wiki/Jodi_Benson" title="Jodi Benson">Jodi Benson</a></td><td style="padding-right:10px;text-align:right;vertical-align:top">3:13</td></tr><tr style="background-color:#f7f7f7"><td style="padding-right:10px;text-align:right;vertical-align:top">6.</td><td style="vertical-align:top">"<a href="/wiki/Under_the_Sea" title="Under the Sea">Under the Sea</a>"</td><td style="vertical-align:top"><a href="/wiki/Samuel_E._Wright" title="Samuel E. Wright">Samuel E. Wright</a></td><td style="padding-right:10px;text-align:right;vertical-align:top">3:12</td></tr><tr style="background-color:#fff"><td style="padding-right:10px;text-align:right;vertical-align:top">7.</td><td style="vertical-align:top">"<a href="/wiki/Part_of_Your_World" title="Part of Your World">Part of Your World</a>" <span style="font-size:85%">(Reprise)</span></td><td style="vertical-align:top"><a href="/wiki/Jodi_Benson" title="Jodi Benson">Jodi Benson</a></td><td style="padding-right:10px;text-align:right;vertical-align:top">2:15</td></tr><tr style="background-color:#f7f7f7"><td style="padding-right:10px;text-align:right;vertical-align:top">8.</td><td style="vertical-align:top">"<a href="/wiki/Poor_Unfortunate_Souls" title="Poor Unfortunate Souls">Poor Unfortunate Souls</a>"</td><td style="vertical-align:top"><a href="/wiki/Pat_Carroll_(actress)" title="Pat Carroll (actress)">Pat Carroll</a></td><td style="padding-right:10px;text-align:right;vertical-align:top">4:49</td></tr><tr style="background-color:#fff"><td style="padding-right:10px;text-align:right;vertical-align:top">9.</td><td style="vertical-align:top">"<a href="/wiki/Les_Poissons" title="Les Poissons">Les Poissons</a>"</td><td style="vertical-align:top"><a class="mw-redirect" href="/wiki/Ren%C3%A9_Auberjonois_(actor)" title="René Auberjonois (actor)">René Auberjonois</a></td><td style="padding-right:10px;text-align:right;vertical-align:top">1:33</td></tr><tr style="background-color:#f7f7f7"><td style="padding-right:10px;text-align:right;vertical-align:top">10.</td><td style="vertical-align:top">"<a href="/wiki/Kiss_the_Girl" title="Kiss the Girl">Kiss the Girl</a>"</td><td style="vertical-align:top"><a href="/wiki/Samuel_E._Wright" title="Samuel E. Wright">Samuel E. Wright</a></td><td style="padding-right:10px;text-align:right;vertical-align:top">2:41</td></tr><tr style="background-color:#fff"><td style="padding-right:10px;text-align:right;vertical-align:top">11.</td><td style="vertical-align:top">"Fireworks" <span style="font-size:85%">(Score)</span></td><td style="vertical-align:top"> </td><td style="padding-right:10px;text-align:right;vertical-align:top">0:38</td></tr><tr style="background-color:#f7f7f7"><td style="padding-right:10px;text-align:right;vertical-align:top">12.</td><td style="vertical-align:top">"Jig" <span style="font-size:85%">(Score)</span></td><td style="vertical-align:top"> </td><td style="padding-right:10px;text-align:right;vertical-align:top">1:32</td></tr><tr style="background-color:#fff"><td style="padding-right:10px;text-align:right;vertical-align:top">13.</td><td style="vertical-align:top">"The Storm" <span style="font-size:85%">(Score)</span></td><td style="vertical-align:top"> </td><td style="padding-right:10px;text-align:right;vertical-align:top">3:18</td></tr><tr style="background-color:#f7f7f7"><td style="padding-right:10px;text-align:right;vertical-align:top">14.</td><td style="vertical-align:top">"Destruction of the Grotto" <span style="font-size:85%">(Score)</span></td><td style="vertical-align:top"> </td><td style="padding-right:10px;text-align:right;vertical-align:top">1:52</td></tr><tr style="background-color:#fff"><td style="padding-right:10px;text-align:right;vertical-align:top">15.</td><td style="vertical-align:top">"Flotsam and Jetsam" <span style="font-size:85%">(Score)</span></td><td style="vertical-align:top"> </td><td style="padding-right:10px;text-align:right;vertical-align:top">1:22</td></tr><tr style="background-color:#f7f7f7"><td style="padding-right:10px;text-align:right;vertical-align:top">16.</td><td style="vertical-align:top">"Tour of the Kingdom" <span style="font-size:85%">(Score)</span></td><td style="vertical-align:top"> </td><td style="padding-right:10px;text-align:right;vertical-align:top">1:24</td></tr><tr style="background-color:#fff"><td style="padding-right:10px;text-align:right;vertical-align:top">17.</td><td style="vertical-align:top">"Bedtime" <span style="font-size:85%">(Score)</span></td><td style="vertical-align:top"> </td><td style="padding-right:10px;text-align:right;vertical-align:top">1:20</td></tr><tr style="background-color:#f7f7f7"><td style="padding-right:10px;text-align:right;vertical-align:top">18.</td><td style="vertical-align:top">"Wedding Announcement" <span style="font-size:85%">(Score)</span></td><td style="vertical-align:top"> </td><td style="padding-right:10px;text-align:right;vertical-align:top">2:16</td></tr><tr style="background-color:#fff"><td style="padding-right:10px;text-align:right;vertical-align:top">19.</td><td style="vertical-align:top">"Eric to the Rescue" <span style="font-size:85%">(Score)</span></td><td style="vertical-align:top"> </td><td style="padding-right:10px;text-align:right;vertical-align:top">3:40</td></tr><tr style="background-color:#f7f7f7"><td style="padding-right:10px;text-align:right;vertical-align:top">20.</td><td style="vertical-align:top">"Happy Ending"</td><td style="vertical-align:top">Disney Chorus</td><td style="padding-right:10px;text-align:right;vertical-align:top">3:11</td></tr><tr><td colspan="3" style="padding:0"><span style="width:7.5em;float:right;padding-left:10px;background-color:#eee;margin-right:2px"><b>Total length:</b></span></td><td style="padding:0 10px 0 0;text-align:right;background-color:#eee"><b>43:18</b></td></tr></tbody></table>
```


위와 같이 내용과 태그가 섞여있는 데이터를 받게 된다. 이제부터 전처리를 하여 표의 내용만 추출할 것이다.
<br><br>
```python
thead = trackListRaw.findAll('th')
tdata = trackListRaw.findAll('td')
```


```python
thead
```


```profile
    [<th class="tlheader" scope="col" style="width:2em;padding-left:10px;padding-right:10px;text-align:right;background-color:#eee"><abbr title="Number">No.</abbr></th>,
     <th class="tlheader" scope="col" style="width:60%;text-align:left;background-color:#eee">Title</th>,
     <th class="tlheader" scope="col" style="width:40%;text-align:left;background-color:#eee">Recording artist(s)</th>,
     <th class="tlheader" scope="col" style="width:4em;padding-right:10px;text-align:right;background-color:#eee">Length</th>]
```


<br><br>

이제 정규표현식을 이용해 전처리를 할 것이다. 먼저 가장 많이 사용할 함수 compile, search, sub의 사용법을 보자.
<br><br>
정규표현식을 컴파일한다.

```python
regex = re.compile('L[a-z]*\sM[a-z]*')
```
<br>
search 함수를 이용하여 컴파일한 정규표현식에 해당하는 부분을 확인할 수 있다.
```python
re.search(regex, 'The Little Mermaid by Walt Disney')
```


```profile
<_sre.SRE_Match object; span=(4, 18), match='Little Mermaid'>
```
<br>

.group()을 붙이면 string에서 해당 부분만 추출할 수 있다.

```python
re.search(regex, 'The Little Mermaid by Walt Disney').group()
```

```profile
'Little Mermaid'
```

<br>

sub 함수는 정규표현식에 해당하는 문자열을 다른 문자열로 대체할 때 사용한다.


```python
re.sub(regex, 'Lion King', 'The Little Mermaid by Walt Disney')
```


```profile
'The Lion King by Walt Disney'
```


<br>


복잡한 데이터에서 태그를 제거하는 것이 목적이므로 태그만 추출하는 정규표현식을 컴파일하여 사용한다. '<[^<]*>'는 <...>와 같은 태그의 형태이면서 그 안에 태그가 아닌 모든 문자열을 포함하는 정규표현식이다. 즉 태그만 추출하여 이를 sub함수로 제거할 것이다. 



```python
regex = re.compile('<[^<]*>')
thList = []
for th in thead:
    thText = re.sub(regex, '', str(th))
    thList.append(thText)
    print(thText)
```
```profile
No.
Title
Recording artist(s)
Length
```
<br>
```python
regex = re.compile('<[^<]*>')
no = []
title = []
artist = []
length = []
i = 0
for td in tdata:
    tdText = re.sub(regex, '', str(td))
    tdText = re.sub('"', '', tdText)
    
    if i % 4 == 0:
        no.append(tdText)
    elif i % 4 == 1:
        title.append(tdText)
    elif i % 4 == 2:
        artist.append(tdText)
    else:
        length.append(tdText)
    i += 1
    print(tdText)
```
```profile
1.
Fathoms Below
Ship's Chorus
1:41
2.
Main Titles (Score)
 
1:26
3.
Fanfare (Score)
 
0:27
4.
Daughters of Triton
Kimmy Robertson, Caroline Vasicek
0:38
5.
Part of Your World
Jodi Benson
3:13
6.
Under the Sea
Samuel E. Wright
3:12
7.
Part of Your World (Reprise)
Jodi Benson
2:15
8.
Poor Unfortunate Souls
Pat Carroll
4:49
9.
Les Poissons
René Auberjonois
1:33
10.
Kiss the Girl
Samuel E. Wright
2:41
11.
Fireworks (Score)
 
0:38
12.
Jig (Score)
 
1:32
13.
The Storm (Score)
 
3:18
14.
Destruction of the Grotto (Score)
 
1:52
15.
Flotsam and Jetsam (Score)
 
1:22
16.
Tour of the Kingdom (Score)
 
1:24
17.
Bedtime (Score)
 
1:20
18.
Wedding Announcement (Score)
 
2:16
19.
Eric to the Rescue (Score)
 
3:40
20.
Happy Ending
Disney Chorus
3:11
Total length:
43:18
```

<br>

데이터를 모두 추출했으므로 pandas를 이용해 웹페이지에 나온 것과 같은 표를 만들어보자.

```python
import pandas as pd
```


```python
mermaid = {'Title': title[:-1], 'Recording artist(s)': artist, 'Length': length}
pd.DataFrame(mermaid, columns=['Title', 'Recording artist(s)', 'Length'])
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
      <th>Title</th>
      <th>Recording artist(s)</th>
      <th>Length</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Fathoms Below</td>
      <td>Ship's Chorus</td>
      <td>1:41</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Main Titles (Score)</td>
      <td></td>
      <td>1:26</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Fanfare (Score)</td>
      <td></td>
      <td>0:27</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Daughters of Triton</td>
      <td>Kimmy Robertson, Caroline Vasicek</td>
      <td>0:38</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Part of Your World</td>
      <td>Jodi Benson</td>
      <td>3:13</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Under the Sea</td>
      <td>Samuel E. Wright</td>
      <td>3:12</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Part of Your World (Reprise)</td>
      <td>Jodi Benson</td>
      <td>2:15</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Poor Unfortunate Souls</td>
      <td>Pat Carroll</td>
      <td>4:49</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Les Poissons</td>
      <td>René Auberjonois</td>
      <td>1:33</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Kiss the Girl</td>
      <td>Samuel E. Wright</td>
      <td>2:41</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Fireworks (Score)</td>
      <td></td>
      <td>0:38</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Jig (Score)</td>
      <td></td>
      <td>1:32</td>
    </tr>
    <tr>
      <th>12</th>
      <td>The Storm (Score)</td>
      <td></td>
      <td>3:18</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Destruction of the Grotto (Score)</td>
      <td></td>
      <td>1:52</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Flotsam and Jetsam (Score)</td>
      <td></td>
      <td>1:22</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Tour of the Kingdom (Score)</td>
      <td></td>
      <td>1:24</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Bedtime (Score)</td>
      <td></td>
      <td>1:20</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Wedding Announcement (Score)</td>
      <td></td>
      <td>2:16</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Eric to the Rescue (Score)</td>
      <td></td>
      <td>3:40</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Happy Ending</td>
      <td>Disney Chorus</td>
      <td>3:11</td>
    </tr>
  </tbody>
</table>
</div>



완성 🙌