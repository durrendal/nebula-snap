# Nebula Snap package

This is an attempt at creating a snap package for the Nebula overlay networking tool.

Current state:

* Nebula binary is running in strict confinement. For this to work you will have to provide:
  * `config.yaml in /var/snap/nebula/common/config`
  * `ca.crt in /var/snap/nebula/common/certs`
  * `nebula-node.crt and nebula-node.key in /var/snap/nebula/common/certs`
* CA creation and certificate signing is working. All nebula-cert commands function, however due to the way snaps works are exposed as `nebula.nebula-cert` instead of just `nebula-cert`.
* Due to strict confinement, `nebula.nebula-cert` can only manipulate certs in `/home` `/mnt` and `/media`

To bypass the above restrictions the snap can be installed with `--devmode`, thereby circumventing the sand boxing in place:

`sudo snap install --devmode nebula`

## Usage

### Starting Nebula
After placing a config.yaml in `/var/snap/nebula/common/config` you should restart nebula using `snap restart nebula`. By default the daemon is enabled and running when you install the snap.

See [here](https://arstechnica.com/gadgets/2019/12/how-to-set-up-your-own-nebula-mesh-vpn-step-by-step/) for instructions on the config file. Also, the [Nebula github page](https://github.com/slackhq/nebula) is a good resource. An example config.yaml can be found there.

#### Start manually:
`sudo nebula`

Due to the strict confinement used with Nebula you must place your config in `/var/snap/nebula/common/config.yaml`. The daemon and run commands for this snap default to these paths. Technically it should be able to tell nebula to use a specific path using the `-path` command, but it will be less seamless and may only work in `--devmode`.

You can validate that your nebula certificates are valid using:
`nebula.nebula-cert print -path /var/snap/nebula/common/certs/node-crt.crt`

Once the configuration is proven, start the snap proper:
`sudo snap start nebula`

To check if the daemon started as expected:
`sudo snap logs nebula`

or using systemd:s logging facilities:
`sudo journalctl -r -u snap.nebula.daemon.service`

### Certificate creation

Aside from needing to use `nebula.nebula-cert` arbitrary flags can be passed to the nebula-cert binary.

`nebula.nebula-cert` additionally has access to the `/home` `/mnt` and `/media` which should ease the process of configuring and validating nebula certificates. Once configured, client certificates for the `nebula` binary will still need to be placed into `/var/snap/nebula/common/certs` for the VPN to operate.

For information on how to generate nebula certificates, please refer to the [official documentation maintained by Defined](https://nebula.defined.net/docs/guides/quick-start/)

# Issues?

Feel free to open an issue on [github](https://github.com/durrendal/nebula-snap) but note that this is only a mirror and as such I might not see the issue right away.
The main repository can be found [here.](https://krei.lambdacreate.com/snap/nebula)

And if you need to reach me, please feel free to reach out to me via email or on oftc or libera.

- Durrendal
