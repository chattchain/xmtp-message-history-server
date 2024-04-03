# XMTP Message History Server

A simple, asynchronous file server provided as an example to support XMTP Message History transfers. 

Running this server allows XMTP service providers to provide the ability for users to securely upload message history bundles via a `POST` request and retrieve them via a `GET` request from another authorized app or device, using a unique ID assigned upon upload.   It is expected that these uploaded bundles should be: encrypted, short-lived, non-guessable, with scoped access to only authorized parties.

## Getting Started

### Prerequisites

Ensure you have the following installed:

- Rust and Cargo. You can install them both from [https://rustup.rs](https://rustup.rs)
- To run the example uploader, ensure python is correctly installed on your system.

Set the `SECRET_KEY` environment variable.

    export SECRET_KEY=super-long-super-secret-unique-key-goes-here
    

### Installing

Clone the repository to your local machine:

    git clone https://github.com/xmtp/xmtp-message-history-server.git
    cd xmtp-message-history-server

Build the project:

    cargo build

Run the server:

    cargo run

The server will start running on http://0.0.0.0:5558.

## Usage

### Uploading a File

To upload a file, send a POST request to http://0.0.0.1:5558/upload with the file data in the request body.  The upload request must include an `X-HMAC` header using `SHA256` as the hashing algorithm.

The server will return a unique ID for the uploaded file.

Example using curl:

    curl -X POST http://0.0.0.0:5558/upload
    -F "file=@path/to/your/message_bundle.aes"
    -H "X-HMAC: <HMAC_VALUE_OF_FILE_SIGNED_WITH_SECRET_KEY>"

Retrieving a File

To retrieve an uploaded file, send a GET request to http://0.0.0.0:5558/files/{id}, where {id} is the unique ID returned by the server during the upload.

Example using curl:

    curl http://0.0.0.0:5558/files/{id} --output retrieved_file.aes

### Example Reference Client  

    cd examples/

Set up a virtual environment

    python3 -m venv myenv
    source myenv/bin/activate

Install the dependencies

    pip3 install -r requirements.txt

Run the uploader script

    python3 upload-bundle.py

Fetch the uploaded file 

    python3 fetch-bundle.py

## Contributing

Contributions are welcome! Please feel free to submit a pull request or open an issue.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
