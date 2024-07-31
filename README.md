# AWS Final Task

## Task 1

**Create Amazon S3 and configure the bucket for the purpose of hosting a static website with public access.**

1. Click on create bucket in the S3 service, enter the bucket name, and choose general purpose.
    
   ![1](https://github.com/user-attachments/assets/3f55483d-a3cd-4068-a926-bc680a4d6fe6)

    
3. Deselect the Block all public access options and acknowledge the warning below.
    
    ![2](https://github.com/user-attachments/assets/e4fc38fc-cad4-4b84-9bfb-19cb97246c85)

    
4. Then, on the properties tab of the bucket we just created click on edit static website hosting and do the following:
    - Enable static website hosting and choose Host a static website.
    - Enter ‘index.html’ as the index document.
    - Save changes.
    
    ![3](https://github.com/user-attachments/assets/acfc2f56-e1a5-4284-8050-67109ff3939d)

5. Now, on the permissions tab click on edit bucket policy and enter the policy:
    
    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Principal": "*",
                "Action": "s3:GetObject",
                "Resource": "arn:aws:s3:::aws-final-task/*"
            }
        ]
    }
    ```
    
    Note that the  `"Resource": "arn:aws:s3:::aws-final-task/*"` section needs to match the name of our S3 bucket.
    
    ![4](https://github.com/user-attachments/assets/050b7a49-b60c-4c00-9e7b-3ab5de83e0aa)


## Task 2

**Create [GitHub](https://github.com/) repo with simple personal website.**

1. Now that we have prepared the S3 bucket for our website we create a new github repository

    ![5](https://github.com/user-attachments/assets/c2a179a5-3d2f-4a26-9ea1-4386f90b8095)
 
    
2. Create and push the index.html file with the following code:
    
    ```html
    <!DOCTYPE html>
    <html lang="en">
      <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>S3 Site</title>
      </head>
      <body>
    
        <h1>
            Hi! 👋 Welcome to My Simple Site
        </h1>
    
        <p>
            This is an Automated Deployment with GitHub Actions. 😄
        </p> 
      </body>
    </html>
    ```
    
    ![6](https://github.com/user-attachments/assets/076607ec-8a62-4a2a-928b-af7466036898)

    
    ![7](https://github.com/user-attachments/assets/fdfb496c-a359-4f1f-a95a-5800cbbd0d81)

    

## Task 3

**Deploy an application with GitHub Actions and Amazon S3.**

1. Choose an existing set of security credentials or create new ones.
    
    ![8](https://github.com/user-attachments/assets/8d62fcfb-c1a6-4c22-bf23-1130b2aec88a)


2. Go to the repository settings and under actions secrets create a new one with the value of the security credentials (access key and security access key)

  ![9](https://github.com/user-attachments/assets/84c947f4-ece1-41c6-b571-623151d7c64d)


After creating both secrets they shoud look like this:

  ![10](https://github.com/user-attachments/assets/78bc9b48-ab75-4e58-b0af-24fd342ca96e)


1. Go to the actions tab and click on set up workflow yourself.
    
    Add the following script to create the workflow:
    
    ```yaml
    name: Upload Website
    
    on:
      push:
        branches:
        - main
    
    jobs:
      deploy-site:
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v2
          - uses: jakejarvis/s3-sync-action@master
            with:
              args: --delete
            env:
              AWS_S3_BUCKET: aws-final-task
              AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
              AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
              SOURCE_DIR: ./
    ```
    
    <aside>
    💡 Note that we have to create enviroment variables on the env section and provide the S3 bucket name, and the security credentials.
    
    </aside>
    
    ![11](https://github.com/user-attachments/assets/4470059c-d2de-4f82-9e26-ac2d8ce08572)

    
    After commiting the main.yml file we should see the first build of our workflow
    
    ![12](https://github.com/user-attachments/assets/97b2c637-0d60-49f1-a1dd-63087c873058)

    
2. Now our S3 bucket should have the files we added to the github repository
    
    ![13](https://github.com/user-attachments/assets/d1c246b3-de4b-4bad-be6c-e43d0952d622)

    
3. Go to the properties tab of your S3 bucket and copy the bucket website endpoint to access our website.
    
    ![14](https://github.com/user-attachments/assets/bea884ff-51aa-420c-a5dd-d997dbf7fb88)


    ![15](https://github.com/user-attachments/assets/92ae9e14-53c6-4a5a-af58-31322977fea7)

    
