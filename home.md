![[98_assets/2025-07-08 21-11-28.jpg|center|150]]  ![[98_assets/IMG_20250517_181030.jpg|center|153]]
 ![[98_assets/IMG_20250613_160322.jpg|center|165]]


---
# Home

[[03_resources/databases/library_db.base|Library]] || [[Список для чтения]] || [[02_areas/MOC - Areas|MOC - Areas]]

## TODO

[[Next actions]] || [[Someday]]

## Inbox

![[03_resources/databases/inbox_db.base]]


## Book


```datacorejsx
// A list of columns to show in the table.
const COLUMNS = [
    { id: "Name", value: page => page.$link },
    { id: "Rating", value: page => page.value("rating") }
];

return function View() {
    // Selecting `#game` pages, for example.
    const pages = dc.useQuery(@page and $class = "book");

    // Uses the built in table component for showing objects in a table!
    return <dc.Table columns={COLUMNS} rows={pages} />;
}
```