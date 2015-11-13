# Migration Guide

*This page is updated to the 2.1.0*

The migration from PuMuKIT1 to PuMuKIT2 allows to export the metadata of series
from PuMuKIT1 and import this metadata into PuMuKIT2. The essence (multimedia
files such as videos and pictures) of these series will not be imported. It is
important that both PuMuKIT1 and PuMuKIT2 servers have access to these essences.

The metadata from Users, Live Channels and Live Events will not be migrated.
For a complete migration, these metadata must be introduced using the back-
-office of PuMuKIT2 (http://{PuMuKIT2-Server}/admin).

## System requirements

Configure your `web/storage/` and `web/uploads/` directories to be mounted on
your PuMuKIT share in a way PuMuKIT1 and PuMuKIT2 servers have access to the
multimedia files.

## Metadata export from PuMuKIT1

There are a series of scripts to export metadata from PuMuKIT1. These scripts
exports all metadata from all the series in PuMuKIT1 database, including its
multimedia objects metadata.

To export this metadata, install these scripts and export the metadata into your
PuMuKIT1 server. In order to do that, follow the installation guide of the [PuMuKIT1-data-export
repository](https://github.com/campusdomar/PuMuKIT1-data-export/blob/master/README.md).

## Metadata import to PuMuKIT2

PuMuKIT2-import-bundle is a Symfony bundle that allows to import the metadata
exported in the format of the PuMuKIT1-data-export repository. This bundle
imports the metadata into the database of PuMuKIT2.

To import this metadata, install this bundle into your PuMuKIT2 instance and
execute the importation following the steps at [PuMuKIT2-import-bundle README](https://github.com/campusdomar/PuMuKIT2-import-bundle/blob/master/README.md).

## Final check

As a recommendation, check you have the same amount of Series and Multimedia
Objects at both systems.

#### PuMuKIT1

```bash
$ mysql -u YOUR_PUMUKIT1.7_USER -p
mysql> use YOUR_PUMUKIT1.7_DATABASE
mysql> select count(*) from serial;
mysql> select count(*) from mm;
```

#### PuMuKIT2

```bash
$ mongo
> use YOUR_PUMUKIT2_DATABASE
> db.Series.count()
> db.MultimediaObject.count()
```
