# Project2 version 2

## wrangle_act.ipynb

- 转发推特的清理
```
twitter_WERATEDOGS_clean=twitter_WERATEDOGS_clean[twitter_WERATEDOGS_clean.retweeted_status_id.isnull()]
twitter_WERATEDOGS_clean = twitter_WERATEDOGS_clean.reset_index(drop=True)
twitter_WERATEDOGS_clean=twitter_WERATEDOGS_clean.drop(['retweeted_status_id','retweeted_status_user_id','retweeted_status_timestamp'],axis=1)
```

- 补充对无图片推特的清理
```
twitter_archive_master=pd.merge(twitter_WERATEDOGS_clean,tweet_data_clean_new,on='tweet_id',how='inner')
twitter_archive_master=pd.merge(twitter_archive_master,image_predictions_clean2,on='tweet_id',how='inner')
```

- image数据集只保留p1
这里的代码很冗余，这是由于我原来使用了melt的方法，按照链接中的解释，只需要保留p1信息，我就在原有代码上取了p1的行
```
#transfer p1, p2, p3 to prediction type
x1=pd.melt(image_predictions_clean,id_vars=['tweet_id','jpg_url','img_num'],value_vars=['p1','p2','p3'],var_name='prediction_type',value_name='result')

#transfer p1_conf, p2_conf, p3_conf to prediction type and value, replace p1_conf to p1...
x3=pd.melt(image_predictions_clean,id_vars=['tweet_id','jpg_url','img_num'],value_vars=['p1_dog','p2_dog','p3_dog'],var_name='prediction_type',value_name='isDog?')
x3['prediction_type']=x3['prediction_type'].replace(['p1_dog','p2_dog','p3_dog'],['p1','p2','p3'])

#transfer p1_dog, p2_dog, p3_dog to prediction type and value, replace p1_dog to p1...
x2=pd.melt(image_predictions_clean,id_vars=['tweet_id','jpg_url','img_num'],value_vars=['p1_conf','p2_conf','p3_conf'],var_name='prediction_type',value_name='confidenceLevel')
x2['prediction_type']=x2['prediction_type'].replace(['p1_conf','p2_conf','p3_conf'],['p1','p2','p3'])

#merge x1, x2, x3 to new table
t1=pd.merge(x1,x2,on=['tweet_id','jpg_url','img_num','prediction_type'])
image_predictions_clean=pd.merge(t1,x3,on=['tweet_id','jpg_url','img_num','prediction_type'])

#only retain rows the prediction_type == p1
image_predictions_clean=image_predictions_clean[image_predictions_clean.prediction_type=='p1']
```


## wangle_report
- 修改了其中需要清理的问题的描述以及修改后的结果图片

## act_report
- 修改了重新清理后的分析。分析的结论不变但是其中的数据稍有变化。