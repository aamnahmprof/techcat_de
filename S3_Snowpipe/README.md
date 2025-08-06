# S3 & Snowpipe Labs

## Lab 1
### Analysis Table
*Analysis of different S3 - Boto3 methods*
|Method|Purpose|Best Use Cases|Pros|Cons|
|------|-------|--------------|----|----|
|`upload_file`|Upload a file to a bucket|Uploading large files from local system|Simple to understand, Allows for multipart uploads and partial uploads|-Only works with local files<br>-Few configuraiton options|
|`upload_fileobj`|Upload a binary file-like object to a bucket|Uploading in-memory objects like streams|Can upload various file types with this method|-Higher memory usage<br>-No automatic path handling|
|`put_object`|Add an entire object to a bucket|Uploading small objects from memory|Allows for more specific configuration with parameters like Access Control List, Checksums, Encyrption Keys, and more|-No multipart or partial uploads<br>-Limited to objects of 5GB size or less|
|`download_file`|Downloads a file from a bucket and saves to the local system|When saving the file for later or using another application to process it|-Straightforwad use<br>-Instant retries & multipart handling|Does not allow in-memory transformation or use after downloading|
|`download_fileobj`|Downloads an object from an S3 bucket to a binary file-like object|When handling data in-memory rather than storing it for later use|-Supports automated multipart downloads<br>-Can download to various file types|-Only works in inary mode<br>-Inconvenient for smaller files|
|`get_object`|Retrieve an object (and its metadata) from an S3 bucket|Accessing object metadata and for smaller files|Parameters allow for more flexible confguration options|-Very slow for large objects<br>-Only usable for one object at a time|

### Reflection Questions
1. Comparison of `upload_file`, `upload_fileobj`, and `put_object`:
   * These methods differ in the files they can handle uploading to an S3 bucket, in terms of size, format, and upload style. `upload_file` and `upload_fileobj` both automatically handle multipart uploads, while `put_object` cannot. This automatically makes `put_object` least suitable for larger file uploads. Additionally, `upload_fileobj` differs from the other two in that it works with files in a binary format only. Overall, `upload_file` and `upload_fileobj` are best suited for larger files, with `upload_fileobj` used when data is in-memory or from an external system. These two methods, however, do not offer nearly the same configuration options as `put_object`.
2. When to use `put_object`:
   * `put_object` should be used for smaller files and when more strict configurations are needed, as the method has many parameters available to be customized as needed. 
3. Comparison of `download_file`, `download_fileobj`, and `get_object`:
   * 
5. When to use `Get_object`:
6. How multipart uploads and downloads enhance the performance of file transfer operations:
7. Limitations of using `put_object` and `get_object` for large files:
   * `put_obect` and `get_object` do not support large files
8. Upload method to use when uploading a large video file to S3 and why:
9. Download method to use to process data in memory before saving it locally:

## Bringing it Together

### Use Cases

### Architecture Diagram
![Architecture Diagram]
