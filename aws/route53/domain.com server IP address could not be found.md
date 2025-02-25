
a static website has been setup in Amazon s3 bucket, a CloudFront distribution has been created with the s3 bucket origin. There is a public hosted zone created. the domain has been set as origin id in CloudFront distribution. A public domain certificate has been created for the domain and CNAME entry for the certificate is made in Route53 hosted zone for the public domain. While we are accessing the domain using browser, it is showing below error.  domain.com’s server IP address could not be found. Please help to fix the issue.


---

### 1. **Verify the Route 53 Hosted Zone Configuration**
   - Ensure that the domain name (`e2esolutionarchitect.com`) is correctly configured in the Route 53 hosted zone.
   - Check if there is an **A record** or **CNAME record** pointing to the CloudFront distribution.
     - For CloudFront, you should use an **A record** with an **Alias** target pointing to the CloudFront distribution domain (e.g., `d12345.cloudfront.net`).
     - Example:
       ```
       Name: e2esolutionarchitect.com
       Type: A
       Alias: Yes
       Alias Target: d12345.cloudfront.net
       ```

---

### 2. **Verify the CloudFront Distribution Configuration**
   - Ensure that the CloudFront distribution is correctly configured:
     - The **Origin Domain Name** should point to the S3 bucket (e.g., `my-bucket.s3.amazonaws.com`).
     - The **Alternate Domain Name (CNAME)** in the CloudFront distribution should match your domain (`e2esolutionarchitect.com`).
     - Ensure that the SSL/TLS certificate for the domain (`e2esolutionarchitect.com`) is attached to the CloudFront distribution.

---

### 3. **Check the SSL/TLS Certificate**
   - Verify that the SSL/TLS certificate for `e2esolutionarchitect.com` is issued and validated in AWS Certificate Manager (ACM).
   - Ensure that the certificate is in the **US East (N. Virginia)** region if it’s being used with CloudFront.
   - Confirm that the CNAME record for domain validation was correctly added to the Route 53 hosted zone.

---

### 4. **Check for Typos or Misconfigurations**
   - Double-check the domain name in the Route 53 hosted zone, CloudFront distribution, and SSL/TLS certificate for any typos or mismatches.

---

### 5. **Test the S3 Bucket and CloudFront Distribution**
   - Access the S3 bucket directly using its endpoint (e.g., `http://my-bucket.s3-website-us-east-1.amazonaws.com`) to ensure the website is working.
   - Access the CloudFront distribution using its default domain (e.g., `https://d12345.cloudfront.net`) to ensure the distribution is working.

---
