# aws_lambda_transcribe_audio

Steps:
Create IAM :
Create an IAM Role
In the search bar, enter "IAM".
Select IAM from the search results.
In the lefthand menu, click Roles.
Click Create role.
Under Choose a use case, select Lambda.
Click Next: Permissions.
In Filter policies, enter "S3".
Select the AmazonS3FullAccess option.
In Filter policies, enter "transcribe".
Select the AmazonTranscribeFullAccess option.
In Filter policies, enter "CloudWatch".
Select the CloudWatchFullAccess option.
Click Next: Tags.
Under Key, enter "app", and under Value, enter "meeting-transcriber".
Click Next: Review.

Create a Lambda Function
In the search bar, enter "lambda".

Select Lambda from the search results.

Click Create functions.

Select the option Author from scratch.

In Function name, enter "transcribe-meeting-lambda".

From the Runtime dropdown, select Python 3.8 (or whatever the latest supported version of Python is available).

Click Change default execution role.

Select Use existing role.

Under Existing role, choose transcribe-meeting-role from the dropdown menu.

Click Create function.

In Function code, input the attached code

Click Deploy.

Create an S3 Bucket
In the search bar, enter "S3".
Select S3 from the search results.
Click Create bucket.
Under Bucket name, enter "meeting-audio-" with some random characters at the end for uniqueness.
Select Block all public access.
In Tags, click Add Tag.
Enter "app" under Key and "meeting-transcriber" under Value.
Under Default encryption, select Enable for Server-side encryption and leave the Encryption key type at the default of Amazon S3 key (SSE-SE).
Click Create bucket.
Click the bucket name.
Click the Properties tab.
Click Create event notification.
In Event name, enter "audio-upload-trigger".
In Suffix (optional), enter ".mp3".
In Event Types, select All object create events.
In the Lambda function dropdown menu, select transcribe-meeting-lambda.
Click Save changes.
Automatically Transcribe Data
Download the ImportantBusiness.mp3 audio file (direct link).
https://raw.githubusercontent.com/linuxacademy/content-aws-mls-c01/master/AutomaticallyProcessS3DataUsingLambda/ImportantBusiness.mp3

Go to the S3 Bucket console.

Click Upload.

Click Add files.

Upload the ImportantBusiness.mp3 file.

Click Upload.

In the search bar, enter "CloudWatch".

Select CloudWatch from the search results.

In the lefthand menu, click Log groups.

Click the /aws/lambda/lambda-meeting-transcribe log group.

Click the log stream.

View the latest event in the log stream and see the details of the event.

In the search bar, enter "Transcribe".

Select Amazon Transcribe from the search results.

Click the job name starting with ImportantBusiness.mp3 and view the details of the job.

In the search bar, enter "S3".

Select S3 from the search results.

Click the bucket whose name starts with meeting-audio.

Click the transcripts/ folder.

Click ImportantBusiness.mp3-transcript.json.

Click on the link under Object URL. This should lead to a page telling us access is denied.

Go back to the transcripts/ folder and check the box next to ImportantBusiness.mp3-transcript.json.

Click Actions.

Select Download from the dropdown menu.

Open a terminal in a new window or tab.

View the job name, account ID, and first 10 words of the audio transcript:

python -m json.tool ~/Downloads/ImportantBusiness.mp3-transcript.json| head -10 
