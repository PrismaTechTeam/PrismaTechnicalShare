## UOM TableList Search

1. Navigate to the following path in the codebase: view → apps → (open folder) UOM.
2. Copy the file named Department.tsx and paste it into the UOM folder.
    
    2.1. Rename the copied file to UOM.tsx.
    
3. Navigate to src → dashboards → modern → TopCard.tsx.
4. Copy the following code block:
    
    ```tsx
    {
        href: /apps/workcentre,
        icon: icon 1,
        title: ‘workcentre’,
        digit: 5,
        bgcolor: info
    }
    ```
    
    
    Paste the copied code block and change the href to /apps/UOM and title to ‘UOM’.
    
5. Navigate to src → components → apps → department.
6. Copy the file named departmentTableList.tsx.
7. Create a new folder named UOM.
8. Paste the copied file into the UOM folder and rename it to UOMTableList.tsx.
9. Go to TopCard.tsx, copy lines 115 to 123 (one of the fetch statements).
10. Get the link from GET API /api/UOM/count and replace the URL in the fetch statement. Change the setCount statement to setUOMCount.
11. Copy one of the lines for the state:
    
    ```tsx
    const [jobOrderCount, setJobOrderCount] = useState<number | null>(null);
    ```
    
    
    Paste it and change to:
    
    ```tsx
    const [UOMCount, setUOMCount] = useState<number | null>(null);
    ```
    
    
12. Copy one of the if statements in const updatedTopCard (below the fetch statement). Change the href to ‘../views/apps/UOM/Uom’ && UOMCount.
13. For routing the TopCard to the pages, go to routes/Router.tsx.
14. Copy the entire block for const Employee under /* ***Apps*** */ and paste it. Change the UOM values to ‘../views/apps/UOM/Uom’.
15. Copy one of the lines for the path, paste it, and change the value to ‘apps/uom’ element: UOM.
16. Navigate to store → apps → department → departmentSlice.
17. Copy and paste into uom, renaming it to UOMSlice.
18. In UOMSlice:
    - Change StateType from department to uom.
    - Change initialState from department to uom.
    - Export const with the name ‘uom’.
    - Below the //GetProduct section, change to getUOM, state.UOM.
19. In endpoints.ts, create a new line:
    
    tsx
    export const urlUOM = ‘${baseURL}/UOM’;
    
    
    
20. In UOMSlice, import urlUOM from endpoints.ts. At the end of the export UOMSlice, change DepartmentSlice.action to UOMSlice.action.
21. Under UOMSlice.action, change fetchDepartment to fetchUOM.
22. In await axios.get(’${}’), change the urlDepartment to urlUOM, AxiosResponse change DepartmentType to UOMType, and in dispatch change getDepartment to getUOM.
23. Navigate to src → types → apps and create a new file named uom.ts.
24. Insert the following code block into uom.ts:
    
    ```tsx
    export interface UOMType {
        id: number;
        uom: string;
        type: string;
    }
    ```
    
    
25. In UOMTableList, import fetchUOM from the UOMSlice.
26. In UOMTableList, in headCells change id and label to Id, Id & UOM, UOM & Type, Type.
27. In UOMTableList, at //FetchProduct dispatch(fetchUOM()) change to fetchUOM. Below:
    
    ```tsx
    const getDepartment: DepartmentType[] = useSelector((state) => state.departmentReducer.department);
    ```
    
    
    Change to:
    
    ```tsx
    const getUOM: UOMType[] = useSelector((state) => state.uomReducer.uom);
    ```
    
    
28. In UOMTableList, in React.useEffect, set rows change the parameter from getDepartment to getUOM.
29. In UOMTableList, change all occurrences of row.department to row.uom, and row.active to row.type.
30. Change departmentReducer.department to uomReducer.uom wherever applicable.
