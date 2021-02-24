---
layout: post
title:  "파이썬 워드클라우드"
subtitle:   "워드 클라우드 작성"
date: 2020-12-26 11:45:51 +0900
categories: study
tags: python
comments: true

---

# 개요
    워드 클라우드 작성 방법

## 1. 코드

    #    텍스트에서 -가 에러나는 경우 그 때 아래 코드가 필요하다
    #    mpl.rcParams['axes.unicode_minus'] = False
    
    #    아래는 절대경로로 이미지를 얻어온다
    #    directory = path.dirname(__file__) if "__file__" in locals() else os.getcwd()
    #    mask = np.array(Image.open(path.join(directory,'pixlr-bg-result.png')))
     
    # 이미지 좌표값을 얻어옴
    mask = np.array(Image.open('pixlr-bg-result.png'))
    read_text = []
    with open('test.csv','r',encoding='utf-8') as fs :
    for line in fs :
        read_text.append(str(line.strip()))
    kkma = Kkma()
    
    list_temp = []
    for row in read_text :
        list_temp = kkma.nouns(row) + list_temp
    #print(list_temp)
    
    list_result = []
    for check in list_temp :
        if len(check) > 1 :
            list_result.append(check)
    # collections.Counter(list_result).most_common(25)
    text_result = ''
    for row in list_result :
        text_result = text_result + ' '+ row
    #NanumGothicCoding.ttf
    
    


    #워드클라우드 객체를 만듬 폰트를 지정하지 않으면 한글이 깨진다 백그라운드는 기본이 검정색
    
    wordcloud = WordCloud(font_path='NanumGothicCoding.ttf',
    background_color='white',
    width = 800,height=500,mask=mask)
    
    wordcloud.generate(text_result)
    
    
    #크기를 지정한다
    plt.figure(figsize=(12,12))
    
    #이미지가 표시됨
    plt.imshow(wordcloud, interpolation='bilinear')
    
    #눈금 제거
    plt.axis('off')
    plt.show()

 
## 2. 참고
    형태소 분석은 Kkma를 사용하였음
    워드클라우드 한글 출력시 한글 폰트를 설정하지 않으면 공백으로 출력되는 문제가 발생한다
