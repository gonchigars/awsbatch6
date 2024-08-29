### Step-by-Step Tutorial to Host a React App in S3 Bucket

#### Prerequisites:
- A React app ready for deployment.
- An AWS account with access to the S3 service.
- AWS CLI installed on your local machine.

### Step 1: Install AWS CLI
1. **Download and Install AWS CLI:**
   - Visit the [AWS CLI official page](https://aws.amazon.com/cli/) to download the installer for your operating system.
   - Run the installer and follow the on-screen instructions.

2. **Verify Installation:**
   - Open the command prompt and run:
     ```bash
     aws --version
     ```
   - Ensure that AWS CLI is installed correctly.

### Step 2: Configure AWS CLI
1. **Open the Command Prompt:**
   - Use the command prompt to configure AWS CLI.

2. **Configure AWS CLI:**
   - Run:
     ```bash
     aws configure
     ```
   - Enter the following when prompted:
     - **AWS Access Key ID**: (Enter your access key ID)
     - **AWS Secret Access Key**: (Enter your secret access key)
     - **Default region name**: `us-east-1`
     - **Default output format**: `json` (or leave it blank)

### Step 3: Build the React App
1. **Navigate to Your React App Directory:**
   - In the command prompt, navigate to the root directory of your React app:
     ```bash
     cd path_to_your_react_app
     ```

2. **Install Dependencies:**
   - Run:
     ```bash
     npm install
     ```

3. **Build the React App:**
   - Run:
     ```bash
     npm run build
     ```
   - This command will create a `build` folder containing your React app's static files.

### Step 4: Create an S3 Bucket
1. **Create an S3 Bucket:**
   - Run:
     ```bash
     aws s3 mb s3://batch6bucket --region us-east-1
     ```

2. **Enable Static Website Hosting:**
   - Run:
     ```bash
     aws s3 website s3://batch6bucket/ --index-document index.html --error-document index.html
     ```

### Step 5: Apply Bucket Policy via AWS Web Console
1. **Log in to AWS Management Console:**
   - Go to the S3 service.

2. **Select Your S3 Bucket:**
   - Find and click on `batch6bucket`.

3. **Go to the Permissions Tab:**
   - Click on the “Permissions” tab.
   - Scroll down to “Bucket policy” and click “Edit”.

4. **Add a Bucket Policy:**
   - Copy and paste the following JSON policy:
     ```json
     {
         "Version": "2012-10-17",
         "Statement": [
             {
                 "Sid": "PublicReadGetObject",
                 "Effect": "Allow",
                 "Principal": "*",
                 "Action": "s3:GetObject",
                 "Resource": "arn:aws:s3:::batch6bucket/*"
             }
         ]
     }
     ```
   - Replace `batch6bucket` with your actual bucket name.

5. **Save the Bucket Policy:**
   - Click “Save changes”.

### Step 6: Uncheck Block Public Access
1. **Go to the Properties Tab:**
   - Still within the `batch6bucket` S3 bucket, navigate to the “Properties” tab.

2. **Edit Block Public Access Settings:**
   - Scroll down to “Block Public Access (Bucket Settings)” and click “Edit”.
   - Uncheck the box labeled “Block all public access”.

3. **Confirm Changes:**
   - Save the changes to allow public access to the bucket.

### Step 7: Deploy the React App to S3
1. **Upload the Build Files to S3:**
   - Run:
     ```bash
     aws s3 sync build/ s3://batch6bucket/ --acl public-read
     ```

### Step 8: Access Your Deployed React App
1. **Get the S3 Website URL:**
   - Your React app should now be accessible via:
     ```plaintext
     http://batch6bucket.s3-website-us-east-1.amazonaws.com/
     ```
