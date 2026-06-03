# Kits

A kit is a self-contained package of configuration and scripts that installs, starts, stops,
and optionally backs up a workload on your cluster. Each kit defines its full lifecycle in a
`kit.yaml` file using typed steps — no Kubernetes YAML wrangling required.

easy-db-lab ships with built-in kits (ClickHouse, Presto). You can also create your own kits
for any workload you want to benchmark or test.

## Discovering kits

List all available kits:

```bash
easy-db-lab kit list
```

Inspect a kit before installing it — see its args, endpoints, and available commands:

```bash
easy-db-lab kit info clickhouse
```

## Installing a kit

```bash
easy-db-lab kit install clickhouse --clickhouse-version 25.4 --size 100Gi
```

Args vary by kit. Run `kit info <name>` to see what a kit accepts, or pass `--help`:

```bash
easy-db-lab kit install clickhouse --help
```

After install, the kit's files are written into a subdirectory of the cluster workspace.
The kit's lifecycle commands are registered automatically.

## Running kit commands

Every installed kit gains a set of subcommands:

```bash
easy-db-lab clickhouse start       # deploy and start the workload
easy-db-lab clickhouse status      # show running state and connection endpoints
easy-db-lab clickhouse stop        # stop and remove the workload
easy-db-lab clickhouse backup --name my-backup   # back up data
easy-db-lab clickhouse restore --name my-backup  # restore from backup
easy-db-lab clickhouse uninstall   # stop and remove all kit resources
```

## Installing a custom kit

Place your kit directory under the profile kits folder and it will appear in `kit list` and be
installable by name like any built-in kit:

```
~/.easy-db-lab/profiles/default/kits/<kit-name>/
```

Custom kits in the profile directory take precedence over built-in kits with the same name.

```bash
mkdir -p ~/.easy-db-lab/profiles/default/kits/my-kit
cp -r /path/to/my-kit/* ~/.easy-db-lab/profiles/default/kits/my-kit/

# Now it appears in kit list and can be installed by name:
easy-db-lab kit install my-kit
```


For a full walkthrough of building and publishing your own kit, see the [Kit Development](../development/kits.md) guide.
