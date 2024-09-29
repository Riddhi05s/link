Deploy a static website using GitHub action and s3 bucket

- Create an ec2 instance
- Create a project repository and index.html

```html
<!DOCTYPE html>
 <html lang="en">
   <head>

 <!-- Declared Vars To Go Here -->
 <meta charset="utf-8">
 <meta http-equiv="X-UA-Compatible" content="IE=edge">
 <meta name="viewport" content="width=device-width, initial-scale=1">

 <!-- Metadata -->
 <meta name="description" content="">
 <meta name="author" content="">

 <link rel="icon" href="mysource_files/favicon.ico">

 <!-- Page Name and Site Name -->
 <title>Page Name - Squiz Matrix HTML Example</title>

 <!-- CSS -->
 <link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css" rel="stylesheet">
 <link href="mysource_files/style.css" rel="stylesheet">

 </head>
 <body>
 <div class="container">
   <header class="header clearfix" style="background-color: #ffffff">

    <!-- Main Menu -->
     <nav>
       <ul class="nav nav-pills pull-right">
         <li class="active"><a href="#">Home</a></li>
         <li><a href="#">About</a></li>
         <li><a href="#">Contact</a></li>
       </ul>
     </nav>

     <!-- Site Name -->
     <h1 class="h3 text-muted">Site Name</h1>

     <!-- Breadcrumbs -->
     <ol class="breadcrumb">
       <li><a href="#">Home</a></li>
       <li><a href="#">Level 1</a></li>
       <li class="active">Level 2</li>
     </ol>

   </header>
   <div class="page-heading">

     <!-- Page Heading -->
     <h1>Page Heading</h1>

   </div>
   <div class="row">

     <div class="col-sm-3">

       <!-- Sub Navigation -->
       <ul class="nav nav-pills nav-stacked">
         <li><a href="#">Level 2</a></li>
         <li class="active"><a href="#">Level 2</a>
           <ul>
             <li><a href="#">Level 3</a></li>
             <li><a href="#">Level 3</a></li>
             <li><a href="#">Level 3</a></li>
           </ul>
         </li>
         <li><a href="#">Level 2</a></li>
       </ul>

     </div>

     <div class="col-sm-6">

       <div class="page-contents">

         <!-- Design Body -->
         <h2>Sub Heading</h2>
         <p>Donec id elit non mi porta gravida at eget metus. Maecenas faucibus mollis interdum.</p>
         <h4>Sub Heading</h4>
         <p>Morbi leo risus, porta ac consectetur ac, vestibulum at eros. Cras mattis consectetur purus sit amet fermentum.</p>
         <h4>Sub Heading</h4>
         <p>Maecenas sed diam eget risus varius blandit sit amet non magna.</p>

       </div>

     </div>

     <div class="col-sm-3">

       <!-- Login Section -->
       <h2>Login</h2>

       <!-- Search Section -->
       <h2>Search</h2>

       <!-- Nested Right Column Content -->

     </div>

   </div>
   <footer class="footer">
     <p class="pull-right">
       <!-- Last Updated Design Area-->
       Last Updated: Wednesday, January 6, 2016
     </p>
     <p>&copy; 2016 Company, Inc.</p>
   </footer>

 </div> <!-- /container -->

 </body>
 </html>
``` 


- Install git on EC2 instance : yum install git -y
- Add ssh-key of EC2 instance to github ssh-keys
- Create one empty github repository
- Upload main.yml in project-repo >> .github >> workflows >> main.yml
```yaml
name: Upload Website

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v3
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ap-south-1

      - name: Deploy static site to S3 bucket
        run: aws s3 sync . s3://riddhi-bucket-githubactions --delete

``` 
      
- Create s3 bucket
- Uncheck block public access
- Enable Static Website Hosting
- Add following permission policy
```json
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Sid": "PublicReadGetObject",
              "Effect": "Allow",
              "Principal": "*",
              "Action": [
                  "s3:GetObject"
              ],
              "Resource": [
                  "arn:aws:s3:::Bucket-Name/*"
              ]
          }
      ]
  }
```

- Add AWS_SECRET_KEY and AWS_ACCESS_KEY in github_repo >> settings >> secret and variables >> actions >> new repository secret
- Add remote origin to the remote repo
- Commit and push code to the github repository
- Code will automatically push to s3 bucket
- Copy obejct URL of index.html and paste in Browser
