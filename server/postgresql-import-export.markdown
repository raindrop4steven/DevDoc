### postgresql

1. export
    
    \copy tablename to 'filename' csv;
    
2. import

    Create your table:

    CREATE TABLE zip_codes 
    (ZIP char(5), LATITUDE double precision, LONGITUDE double precision, 
    CITY varchar, STATE char(2), COUNTY varchar, ZIP_CLASS varchar);
    Copy data from your CSV file to the table:
    
    COPY zip_codes FROM '/path/to/csv/ZIP_CODES.txt' DELIMITER ',' CSV;