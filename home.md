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
const COLUMNS = [
  { id: "Name", value: (row) => row.$name },
{ id: "Link", value: (row) => row.$link },
{ id: "Path", value: (row) => row.$path }
]

return function View() {
const pages = dc.useQuery("@page and class = book");

// 1. Сортируем данные по имени в алфавитном порядке
const sortedPages = dc.useArray(pages, array =>
array.sort(page => page.$name)
);
const [filter, setFilter] = dc.useState("");

    // Замеморизованное значение для отсортированных и отфильтрованных команд.
    // Это предотвращает пересчет при каждом рендере, если фильтр не меняется.
const filteredPages = dc.useMemo(() => {
        // 2. Если фильтр не применен, возвращаем отсортированные данные
        if (!filter) {
            return sortedPages;
        }
        console.log(pages);
        // 3. Фильтруем по имени или id
        return sortedPages.filter((obj) => 
obj.$name?.toLowerCase().includes(filter.toLowerCase()) ||
obj.$link?.path.toLowerCase().includes(filter.toLowerCase())
);
    }, [filter]); // Этот хук перезапускается только при изменении 'filter'

return (
<div>
<input
type="text"
placeholder="Search..."
value={filter}
onChange={(e) => setFilter(e.target.value)}
/>

<dc.Table rows={filteredPages} paging={10} columns={COLUMNS} />
</div>
);
}
```