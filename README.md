![Torq - Banner](./docs/images/readme-banner.png)

# Torq

![All Tests](https://github.com/lncapital/torq/actions/workflows/test-on-push.yml/badge.svg)

Torq is an advanced node management software that helps lightning node operators analyze and automate their nodes. It is designed to handle large nodes with over 1000 channels, and it offers a range of features to simplify your node management tasks, including:

* Analyze, connect and manage all your nodes from one place!
* Access a complete overview of all channels instantly.
* Build advanced automation workflows to automate Rebalancing, Channel Policy, Tagging and eventually any node action.
* Review forwarding history, both current and at any point in history.
* Customize and save table views. Containing only selected columns, advanced sorting and high fidelity filters.
* Export table data as CSV. Finally get all your forwarding or channel data as CSV files.
* Enjoy advanced charts to visualize your node's performance and make informed decisions.

Whether you're running a small or a large node, Torq can help you optimize its performance and streamline your node management process. Give it a try and see how it can simplify your node management tasks.

![torq-automation-preview](./docs/images/automation.png)


## Quick start

### Docker compose
To install Torq via docker compose:

```bash
bash -c "$(curl -fsSL https://torq.sh)"
```
You do not need sudo/root to run this, and you can check the contents of the installation script here: https://torq.sh

When you:
 - Have a firewall
 - Run Torq in a container
 - Need to access LND or CLN on the host
 - Are not using host network configuration for the container

Then make sure to allow docker bridge network traffic i.e. `sudo ufw allow from 172.16.0.0/12`

### Podman
To run the database via host network:

```sh
podman run -d --name torqdb --network=host -v torq_db:/var/lib/postgresql/data -e POSTGRES_PASSWORD="<YourPostgresPasswordHere>" timescale/timescaledb:latest-pg14
```

To run Torq via host network:

First create your TOML configuration file and store it in `~/.torq/torq.conf`

```sh
podman run -d --name torq --network=host -v ~/.torq/torq.conf:/home/torq/torq.conf lncapital/torq:latest --config=/home/torq/torq.conf start
```
**Note**: Only run with host network when your server has a firewall and doesn't automatically open all port to the internet. You don't want the database to be accessible from the internet!

### Kubernetes

We shared templates for CRDs in folder [kubernetes](./kubernetes).
This folder also has its own [readme](./kubernetes/README.md).

### Network

Be aware that when you try Torq on testnet, simnet or some other type of network that you must use the network switch when trying to browse the web interface.
The network switch is the globe icon in the top left corner, next to the Torq logo.


### Guides

We're adding more guides and help articles on [https://docs.torq.co](docs.torq.co).

* [How to add a domain for Torq with https](https://docs.torq.co/en/articles/7323907-how-to-add-a-domain-to-torq-using-caddy).
* [How to monitor your infrastructure with Torq](https://docs.torq.co/en/articles/7323908-how-to-monitor-your-infrastructure-with-torq).

## Configuration

Torq supports a TOML configuration file. The docker compose install script auto generates this file.
You can find an example configuration file at [example-torq.conf](./docker/example-torq.conf)

It is also possible not to use any TOML configuration files and use command like parameters or environment variables. The list of parameters are:
 - **--lnd.url**: Host:Port of the LND node (example: "127.0.0.1:10009")
 - **--lnd.macaroon-path**: Path on disk to LND Macaroon (example: "~/.lnd/admin.macaroon")
 - **--lnd.tls-path**: Path on disk to LND TLS file (example: "~/.lnd/tls.cert")
 - **--cln.url**:  Host:Port of the CLN node (example: "127.0.0.1:17272")
 - **--cln.certificate-path**: Path on disk to CLN client certificate file (example: "~/.cln/client.pem")
 - **--cln.key-path**: Path on disk to CLN client key file (example: "~/.cln/client-key.pem")
 - **--cln.ca-certificate-path**: Path on disk to CLN certificate authority file (example: "~/.cln/ca.pem")
 - **--db.name**: Name of the database (default: "torq")
 - **--db.user**: Name of the postgres user with access to the database (default: "postgres")
 - **--db.password**: Password used to access the database (default: "runningtorq")
 - **--db.port**: Port of the database (default: "5432")
 - **--db.host**: Host of the database (default: "localhost")
 - **--torq.password**: Password used to access the API and frontend (example: "C44y78A4JXHCVziRcFqaJfFij5HpJhF6VwKjz4vR")
 - **--torq.network-interface**: The nework interface to serve the HTTP API (default: "0.0.0.0")
 - **--torq.port**: Port to serve the HTTP API (default: "8080")
 - **--torq.pprof.path**: When pprof path is set then pprof is loaded when Torq boots. (example: ":6060"). **See Note**
 - **--torq.prometheus.path**: When prometheus path is set then prometheus is loaded when Torq boots. (example: "localhost:7070"). **See Note**
 - **--torq.debuglevel**: Specify different debug levels (panic|fatal|error|warn|info|debug|trace) (default: "info")
 - **--torq.vector.url**: Alternative path for alternative vector service implementation (default: "https://vector.ln.capital/")
 - **--torq.cookie-path**: Path to auth cookie file
 - **--torq.no-sub**: Start the server without subscribing to node data (default: "false")
 - **--torq.auto-login**: Allows logging in without a password (default: "false")
 - **--customize.mempool.url**: Mempool custom URL (no trailing slash) (default: "https://mempool.space")
 - **--otel.exporter.type**: (optional) OpenTelemetry exporter type: stdout/file/jaeger (default: "stdout")
 - **--otel.exporter.endpoint**: (optional) OpenTelemetry exporter endpoint
 - **--otel.exporter.path**: (optional) OpenTelemetry exporter path (default: "traces.txt")
 - **--otel.sampler.fraction**: (optional) OpenTelemetry sampler fraction (default: "0.0")
 - **--bitcoind.network**: (optional) Bitcoind network: MainNet/TestNet/RegTest/SigNet/SimNet. (default: "MainNet")
 - **--bitcoind.url**: (optional) Bitcoind RPC Host:Port
 - **--bitcoind.user**: (optional) Bitcoind RPC username
 - **--bitcoind.password**: (optional) Bitcoind RPC password

**Note**: pprof and prometheus expose internal statistics, be careful not to expose this publicly.

More information about infrastructure and node monitoring over [here](https://docs.torq.co/en/articles/8488866-infrastructure-and-node-monitoring)

## How to Videos

[You can find the full list of video guides here.](https://docs.torq.co/en/collections/3817618-torq-video-tutorials)

### How to create custom Channel Views

[![Torq Forwarding Views YouTube Guide](https://img.youtube.com/vi/5ZfgflfOFwQ/maxresdefault.jpg)](https://www.youtube.com/watch?v=5ZfgflfOFwQ)

### How to use Automation Workflows

[![Torq Workflow Automation YouTube Guide](https://img.youtube.com/vi/Go4uJoMhwrE/maxresdefault.jpg)](https://www.youtube.com/watch?v=Go4uJoMhwrE)

### How to use the Forwards Tab

[![Torq Forwarding Views YouTube Guide](https://img.youtube.com/vi/ZTetH8_jbgk/maxresdefault.jpg)](https://www.youtube.com/watch?v=ZTetH8_jbgk)


## LND Permissions

Since Torq is built to manage your node, it needs most/all permissions to be fully functional. However, if you want to
be extra careful you can disable some permissions that are not strictly needed.

Torq does not for now need the ability to create new macaroon or stop the LND daemon,

    lncli bakemacaroon \
        invoices:read \
        invoices:write \
        onchain:read \
        onchain:write \
        offchain:read \
        offchain:write \
        address:read \
        address:write \
        message:read \
        message:write \
        peers:read \
        peers:write \
        info:read \
        uri:/lnrpc.Lightning/UpdateChannelPolicy \
        --save_to=torq.macaroon

Here is an example of a macaroon that can be used if you want to prevent all actions that sends funds from your node:

    lncli bakemacaroon \
        invoices:read \
        invoices:write \
        onchain:read \
        offchain:read \
        address:read \
        address:write \
        message:read \
        message:write \
        peers:read \
        peers:write \
        info:read \
        uri:/lnrpc.Lightning/UpdateChannelPolicy \
        --save_to=torq.macaroon

## CLN

We support CLN nodes. Make sure your CLN node is up-to-date.

You will have to have RUST active and also specify  `--grpc-port` which should generate the appropriate mTLS certificates.
You need to provide these certificates once Torq is running (or as boot parameter or in the configuration file)

## Compatibility

Torq `v0.22.1` -> `v1.1.5` are all compatible with `CLN v23.05.*`

Torq `v1.2.0` and up are compatible with `CLN v23.08.1+` so **NOT** with `CLN v23.05.*`

## Help and feedback

Join our [Telegram group](https://t.me/joinchat/V-Dks6zjBK4xZWY0) if you need help getting started.
Feel free to ping us in the telegram group if you have any feature request or feedback.  We would also love to hear your ideas for features or any other feedback you might have.
