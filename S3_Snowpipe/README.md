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
   * These methods differ in the files they can handle uploading to an S3 bucket, in terms of size, format, and upload style. `upload_file` and `upload_fileobj` both automatically handle multipart uploads, while `put_object` cannot. This automatically makes `put_object` least suitable for larger file uploads. Additionally, `upload_file` differs from the other two in that it is the only one that works with files in a non-binary format. Overall, `upload_file` and `upload_fileobj` are best suited for larger files, with `upload_fileobj` used when data is in-memory or from an external system. These two methods, however, do not offer nearly the same configuration options as `put_object`.
2. When to use `put_object`:
   * `put_object` should be used for smaller files and when more strict configurations are needed, as the method has many parameters available to be customized as needed. It should also be used when uploading un-structured objects without converting them first.
3. Comparison of `download_file`, `download_fileobj`, and `get_object`:
   * `download_file` and `download_fileobj` each download files and have automated multipart handling, but do have different specific purposes. `download_file` will just take the desired file and write it to a local, given path. `download_fileobj` writes to file-like objects that can be used for in memory processing. On the other hand, `get_object` is ideal for smaller objects and is best suited for getting metadata and an in-memory stream for processing.
4. When to use `Get_object`:
   * `get_object` should be used when retrieving objects without needing or wanting a structured format. This allows for more direct file processing.
5. How multipart uploads and downloads enhance the performance of file transfer operations:
   * Multipart uploads and downloads allow a large file to be processed in smaller chunks simultaneously. It also allows for termination, pausing, and resumation of any chunk - so, if something goes wrong with one section of data, it does not affect the entire file. The parallel nature of this significantly speeds up upload/download efficiency in various ways.
6. Limitations of using `put_object` and `get_object` for large files:
   * `put_obect` and `get_object` do not support large files
7. Upload method to use when uploading a large video file to S3 and why:
    * `put_object` does not handle large files or use multipart handling, while both `upload_file` and `upload_fileobj` do. They work with different datastreams, so the storage location of the file determines which of the two methods should be used:
        * `upload_file` if stored locally
        * `upload_fileobj` if stored in-memory
8. Download method to use to process data in memory before saving it locally:
    * `download_fileobj`

## Bringing it Together
### Use Cases
1. **Frequent & Repeated Data Ingestion Pipeline**: The data can be converted and made available for Snowflake querying automatically and quickly.
2. **Cost Effective Data Storage**: Allows querying through Snowflake while storing raw and processed files in S3 buckets (best of both worlds, in a way)
3. **Historic Data Storage**: Metadata is stored permanently in the Glue Catalog, so lifetime policies can be used to transition older files to lower-cost storage without making the data unavailable

### Architecture Diagram
![Architecture Diagram](https://github.com/aamnahmprof/techcat_de/blob/main/S3_Snowpipe/S3%20and%20Snowpipe.drawio%20(1).png)
