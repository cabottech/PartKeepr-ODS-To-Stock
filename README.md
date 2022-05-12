# PartKeepr-ODS-To-Stock

A Python script that processes ODS (spreadsheet) files and imports stock numbers into PartKeepr. Intended to help automate parts received from suppliers.

- Author: [Cabot Technologies](https://cabottechnologies.com)
- Licence: MIT (see the LICENSE file)

The following details how we intend this script to operate. This may change during development.

## Theory of operation

1. When run, this script will check a defined path for any new ODS files. The ODS file *must* be in the correct format/layout. An example ODS file will be provided.
2. If a new ODS is found, the script will read the file data for processing:
    a. The order reference and date-time is read. This will be used when adjusting stock levels for each part in PartKeepr.
    b. Each part row is processed as follows:
        - The IPN field is checked for a match in PartKeepr. If no match is found, we skip the row.
        - The Qty field (positive or negative number) is checked. If zero (0), we skip the row.
        - The stock Qty is entered into the PartKeeper database with the order reference and date-time (from step 2).
        - Upon success, the number is written to the Imported field in the ODS file. This allows the user to confirm which parts were successfully imported.
3. The ODS filename is appended with "-success" if there were no errors, or "-failed" if there were errors.
4. The ODS file is then moved to the "imported/" sub-directory.
5. The script will then repeat from step 2 until no more ODS files are found.

## Limitiations

- Only existing parts are supported. If a part isn't found, it will *not* be created. Any missing part errors will be logged so they don't go unnoticed by the user.
- Each part requires a unique identifier. We have used an "IPN" (internal part number) to match our inhouse workflow. It may be possible to use an PartKeepr UID or MPN by modifying the script.

## Configuration

- PartKeepr database:
    - host/server address
    - username
    - password
- File paths:
    - Source path
        - Where to find new ODS files for processing.
        - Note: The script does not look in sub-directories.
    - Imported path
        - Where to put files after they have been processed.
        - This should be different from the source path.

## Future plans

Once we have this working, we may make this a web-app in future for convenience. I.e. run on the PartKeepr server (on a different port), allow ODS file upload, or entry into table, and proccessing all in the browser.

