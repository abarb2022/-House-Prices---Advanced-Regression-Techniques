# -House-Prices---Advanced-Regression-Techniques

 - პროექტის მიმოხილვა
პროექტი არის Kaggle-ის "House Prices - Advanced Regression Techniques" კონკურსი, რომელშიც უნდა დავაპროგნოზიროთ სახლების საბოლოო ფასი 79 feature-ს საფუძველზე. პროექტში გამოვიყენე Cleaning, Feature Engineering და Feature Selection - ის სხვადასხვა მიდგომები, რათა ეფექტურად მოიხდეს სხვადასხვა ტიპის მონაცემების (რიცხვითი და კატეგორიული) და Nan მნიშვნელობების დამუშავება.

- ჩემი მიდგომა: მონაცემების გატესტვა და დამუშავება, რის შემდეგაც დავტესტე მოდელის დატრენინგების 4 სხვადასხვა მეთოდი: LinearRegression, RandomForest, XGBoosting და GradientBoosting

 - რეპოზიტორიის სტრუქტურა
model_experiment.ipynb - ყველა ეტაპი ( Cleaning, Feature Engineering, Feature Selection და Training ის ნაწილები) და შესაბამისი დალოგვა არის ამ ფაილში.
model_inference.ipynb - ამ ფაილში  ხდება test set ზე პროგნოზი, საუკეთესო მოდელის გამოყენებით  ვაგენერირებ submissions. ჩემს Linear მოდელს ვალოუდებ  Model Registryდან.

 - Feature Engineering - 
მონაცემების nullებისგან გასათავისუფლებლად მაქვს ცალკე NullCLeaner კლასი(გადავცემთ null-ის შევსების "სტრატეგიას"), რომეშიც რიცხვითი სვეტები: შევსებულია საშუალოთი, ხოლო კატეგორიული სვეტები: შევსებულია მოდით. ვშლი ისეთ სცეტებს, რომელში 80 პროცენტზე მეტი null იყო. ასევე ზოგ სპეციფიურ სვეტში null-ებს ვანაცვლებ 0-ებით, რადგან რიგ შემთხვევებში null ნიშნავს features არ ქონას.
CustomPreprocessor, აქ ვჰენდლავ numerical მნიშვნელობებს. 3-ზე ნაკლები უნიკალურ მნიშვნელობიანი სვეტებისთვის ვიყენებ one-hot encodingს. ხოლო მეტიანებზე გამოვიყენე Ordinal Encoding. 

 - Feature Selection -
გამოვიყენე RFE, რომელიც ტოვებს მხოლოდ ტოპ 60 feature-ს, ეს რიცხვი გრიდ სერჩით ვიპოვე, კარგ შედეგს 60 feature-ს დატოვება დებდა, ამასთან ვცადე მაღალ კოლერილებული featurebის პოვნა და გაფილტვრა. ამან დიდად არ გააუმჯობესა შედეგი, ამიტომ არ ვიყენებ, რაც დიდი ალბათობით RFE-ს დამსახურებაა. 

 - Training -
დავტესტე მოდელის დატრენინგების 4 სხვადასხვა მეთოდი: LinearRegression, RandomForest, XGBoosting და GradientBoosting. ეს ოთხი მოდელი გაიტესტა 5-fold CV და  RMSE-ის გამოყენებით.
მათი საუკეთესო შედეგები ჩემ მიერ გამოყოფილ ტესტ სეტზე:
Linear - 0.14863091278842672
ranodm forest - 0.15002109375431913
gradient boosting - 0.1390038465702412
xgb - 0.13674433095800784

 - Hyperparameter ოპტიმიზაციის მიდგომა - GridSearch გამოვიყენე მხოლოდ Scalers შესამოწმებლად (StandardScaler, MinMaxScaler, None). მაგალითად Linear-ისთვის  StandardScaler() იყო საუკეთესო (დალოგილი მაქვს თითოეულისთვის საუკეთესო sclaer).
ყველაზე კარგი შედეგი xgb-მ აჩვენა, თუმცა საბოლოო მოდელიად Linear დავტოვე, რადგან საბოლოო შედეგთან ერთად ვამოწმებდი train set-ისა და validation-ის ერთმანეთთან კორელაციას, რათა მაქსიმალურად ამერიდებიდა overfit/underfit. ყველაზე დაბალანსებული შედეგი კი linerRegression-მა აჩვენა, დანარჩენს პატარა ცდომილებები ჰქონდა და მიდიოდა overfitსკენ.
random forest - training error much lower than validation error
gradient regression -Training error much lower than validation error
xgb - Training error much lower than validation error


 - MLflow Tracking -
4ვე ექსპერიმენტი დალოგილი მაქვს MLflow-ზე : [ https://dagshub.com/abarb22/-House-Prices---Advanced-Regression-Techniques.mlflow/#/experiments/0?searchFilter=&orderByKey=attributes.start_time&orderByAsc=false&startTime=ALL&lifecycleFilter=Active&modelVersionFilter=All+Runs&datasetsFilter=W10%3D ]

 - პარამეტრები -  მოდელის ტიპი, Feature შერჩევის მეთოდი, საუკეთესო სკალერი.
 - მეტრიკები: ტრენინგის/ვალიდაციის/ტესტის RMSE და მათ შორის ცდომილებები.


 - Linear - https://dagshub.com/abarb22/-House-Prices---Advanced-Regression-Techniques.mlflow/#/experiments/0/runs/8f875aac68bb4d5a9563f1db7329c782 
 - ranodm forest - https://dagshub.com/abarb22/-House-Prices---Advanced-Regression-Techniques.mlflow/#/experiments/0/runs/e6956aee23a142858d64035b2cdad540
 - gradient boosting - https://dagshub.com/abarb22/-House-Prices---Advanced-Regression-Techniques.mlflow/#/experiments/0/runs/ed84742aa5304a858acd06f968b68a1c
 - xgb - https://dagshub.com/abarb22/-House-Prices---Advanced-Regression-Techniques.mlflow/#/experiments/0/runs/70ba28b7d07e4688a699cc76718c37c6




