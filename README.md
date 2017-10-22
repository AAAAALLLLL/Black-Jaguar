# Black-Jaguar
install.packages("caret")
library(caret)
training<-read.csv("E:\\pml-training.csv")
testing<-read.csv("E:\\pml-testing.csv")
View(training)
set.seed(1846)
ncol(training)
nearzero <- nearZeroVar(training, saveMetrics = TRUE)
training <- training[,!nearzero$nzv]
ncol(training)
rvar<-sapply(colnames(training),function(x)if(sum(is.na(training[,x]))>0.5*nrow(training)){return(TRUE)}else{return(FALSE)})
training<-training[,!rvar]
View(training)
training<-training[,-c(1:6)]
ncol(training)
c<-findCorrelation(cor(training[,-53]),cutoff = 0.85)
names(training[c])
c<-findCorrelation(cor(training[,-53]),cutoff = 0.75)
names(training[c])
tc<-trainControl(method = "repeatedcv",number = 5,preProcOptions = "pca")
svmradial<-train(classe~.,data = training,method="svmRadia",trControl=tc)
confusionMatrix(svmradial)
svmlinear<-train(classe~.,data = training,method="svmLinear",trControl=tc)
confusionMatrix(svmlinear)
install.packages("nnet")
library(nnet)
nn<-train(classe~.,data = training,method="nnet",trControl=tc)
confustionMatrix(nn)
tree<-train(classe~.,data = training,method="rpart",trControl=tc)
confusionMatrix(tree)

psvml<-predict(svmlinear,testing)
psvmr<-predict(svmradial,testing)
pnn<-predict(nnet,testing)
ptree<-predict(tree,testing)


psvmr
