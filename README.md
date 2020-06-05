# Cape Python API

The Python API's main goal is to allow data scientists to query data from the
Cape data connector and easily use the resulting data in familiar python data science tools.

## Getting Started

A Cape Coordinator and Data Connector must first be setup before getting started with the Python API.
Follow the instructions in the [README.md](https://github.com/capeprivacy/cape/blob/master/README.md).

Next, make sure you have at least Python 3.6 installed. We recommend running it in a virtual environment
such as with [venv](https://docs.python.org/3/library/venv.html) or
[conda](https://www.anaconda.com/products/individual).

`make` will also be required to run our `Makefile` so ensure that you have that installed as well.

### Bootstrapping

Bootstrapping your environment installs all direct dependencies of the Python API
and ensures that the API is installed as well.

Run the following command from a command line:

```bash
$ make bootstrap
```

### Using the Cape Python API

Using the Python API is quite simple. Inside of a juypter notebook or python script copy and paste the following code:

```python
import cape

cl = cape.Client("<COORDINATOR URL>", root_certificates="<CAPE REPO>/connector/certs/localhost.crt")

cl.login("<API TOKEN>")

stream = cl.pull("creditcards", "SELECT * FROM transactions", 50, 0)

df = stream.to_pandas()

print(df.head())
```

Replace `API TOKEN` above with the token gathered in the [Get a API Token](#get-a-api-token) section.

Replace `<CAPE REPO>` with the directory location of the Cape repo so that TLS and grpc work properly.

If all went well you should see the first few rows printed out as a pandas data frame.

#### Get an API Token

To authenticate with coordinator and the connector you must get an API token. For now, this must be done
via the Cape CLI. Assuming you set up a Cape environment from the
[README.md](https://github.com/capeprivacy/cape/blob/master/README.md) you can create a token with:

```bash
$ cape tokens create <identifier>
```

where `<identifier>` is replaced with the email you used during Cape environment setup.

This command will print out a token which can then be copied and pasted into
the [code section above](#using-the-cape-python-api).
