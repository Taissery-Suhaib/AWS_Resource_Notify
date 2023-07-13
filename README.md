# AWS_Resource_Notify_Using_AWS_LAMBDA


![image](https://github.com/Taissery-Suhaib/AWS_Resource_Notify/assets/134582331/4a1cc6d8-4474-420e-ba76-1eb840cb6c01)

---------------------------------------------------------------------------------------------------------------------- 


**Triggering** : triggering the event using a ***Cloud_Watch*** with a scheduled for every day at 8:0 clock UTC

**Destination** : we set a destination as a SNS Topic which will send a notification to service "we have used email as source"


_______________________________________________________________________________________________________________________________
                                                       ***LAMBDA_CODE***
stage 1 :

* > create a def function using a boto3 synstex for *EC2*
* > create a for loop for INSTANCE with the parameters required ( like : ***type*** , ***ID***,***State*** and append it to resource [] )
* > repeate the step for all the resource which required __ For ( VPC , EIP , etc...)

stage 2 :

* > Give a Def function for SNS Topic with ***SNS_Boto3*** and specify the SNS Topic ARN
* > with a message subject

Stage 3 :

* > ***CALLING = LAMBDA_HANDLER***
* > get resource as assigning as a variable from the stage 1
* > specify in the resource so it can call resource [] .
* > with a syntax of summary for message
* > specify the syntax for subject and SNS

________________________________________________________________________________________________________________________
                                                 ***OUT_PUT***
![image](https://github.com/Taissery-Suhaib/AWS_Resource_Notify/assets/134582331/9b159bb4-48ba-4589-b565-108ad1ddff40)

