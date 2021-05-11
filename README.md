# React Folder Tree
A powerful and customizable react treeview library. It supports:
- ✅ inline CRUD operations
- ✅ customizable icons
- ✅ customizable callbacks
- ✅ half check (indeterminate checkboxes)
## Quick Preview
![folder-tree-demo](/assets/folder-tree-demo.gif)


## Demos & Code Examples
[===== HERE =====](https://shunjizhan.github.io/react-folder-tree-demos/)
[===== HERE =====](https://shunjizhan.github.io/react-folder-tree-demos/)
[===== HERE =====](https://shunjizhan.github.io/react-folder-tree-demos/)

---
## Basic Usage
### 🌀 install
```bash
$ yarn add react-folder-tree
$ npm install react-folder-tree --save
```
### 🌀 basic tree
```tsx
import FolderTree, { testData } from 'react-folder-tree';

const BasicTree = () => {
  const onTreeStateChange = state => console.log('tree state: ', state);

  return (
    <FolderTree
      data={ testData }
      onChange={ onTreeStateChange }
    />
  );
};
```

### 🌀 custom initial state
tree state is an object that look like:
```jsx
{
  name: 'Goku',   
  checked (optional): 0 (unchecked, default) | 0.5 (half checked) | 1(checked),
  isOpen (optional): false (default) | true,
  children (optional): [array of treenode],

  // WIP
  clickable (optional): true (default) | false,
  editable (optional): true (default) | false, 
  deletable (optional): true (default) | false,
  canAddFile (optional): true (default) | false,
  canAddFolder (optional): true (default) | false,
}
```
```tsx
const treeState = {
  name: 'root [half checked and opened]',
  checked: 0.5,   // half check: some children are checked
  isOpen: true,   // this folder is opened, we can see it's children
  children: [
    { name: 'children 1 [not checked]', checked: 0 },
    {
      name: 'children 2 [half checked and not opened]',
      checked: 0.5,
      isOpen: false,
      children: [
        { name: 'children 2-1 [not checked]', checked: 0 },
        { name: 'children 2-2 [checked]', checked: 1 },
      ],
    },
  ],
};

const CustomInitState = () => (
  <FolderTree
    data={ treeState }
    initCheckedStatus='custom'  // default: 0 [unchecked]
    initOpenStatus='custom'     // default: 'open'
  />
);
```

### 🌀 hate checkbox?
```jsx
<FolderTree
  data={ treeState }
  showCheckbox={ false }    // default: true
/>
```

### 🌀 love indentation?
```jsx
<FolderTree
  data={ treeState }
  indentPixels={ 99999 }    // default: 30
/>
```

---
## Advanced Usage
### 🌀 sync tree state
in order to perform more complex operations, we can sync and keep track of the current tree state outside.

```jsx
// this example shows how to download all selected files
const SuperApp = () => {
  const [treeState, setTreeState] = useState(initState);
  const onTreeStateChange = state => setTreeState(state);

  const onDownload = () => downloadAllSelected(treeState);

  return (
    <>
      <FolderTree
        data={ initState }
        onChange={ onTreeStateChange }
      />
      <DownloadButton onClick={ onDownload } />
    </>
  );
};
```

### 🌀 custom icons 
there are 9 icons and all of them are customizable.
- FileIcon
- FolderIcon
- FolderOpenIcon
- EditIcon
- DeleteIcon
- CancelIcon
- CaretRightIcon
- CaretDownIcon
- OKIcon

```jsx
// this example shows how to customize the FileIcon
// all other icons have same interface
import { FaBitcoin } from 'react-icons/fa';

const BitcoinApp = () => {
  const FileIcon = ({ onClick: defaultOnClick, className, path, name }) => {
    /* 
      `path` is an array of indexes from root to the node that was clicked.
      It can be used with the synced tree state outside to find all data about the clicked node.
    */
    const handleClick = () => {   
      doSthBad({ className, path, name });
      defaultOnClick();
    };

    return <FaBitcoin onClick={ handleClick } />;
  };

  return (
    <FolderTree
      data={ initState }
      iconComponents={{
        FileIcon,
        /* other custom icons ... */
      }}
    />
  );
};
```

### 🌀 disable icons
this usage is a subset of custom icons. For example, to hide `FileIcon` we can simply pass in a dummy custom icon
```jsx
const FileIcon = (...args) => null;
```

### 🌀 custom `onClick` for node names
WIP

---
## APIs Summary

| prop              | description                             | type     | options                                        |
|-------------------|-----------------------------------------|----------|------------------------------------------------|
| data              | initial tree state data (required)      | object   | N/A                                            |
| onChange          | callback when tree state changes        | function | console.log (default)                          |
| initCheckedStatus | initial check status of all nodes       | string   | 'unchecked' (default) \| 'checked' \| 'custom' |
| initOpenStatus    | initial open status of all treenodes    | string   | 'open' (default) \| 'close' \| 'custom'        |
| iconComponents    | custom icon components                  | object   | N/A                                            |
| indentPixels      | ident pixels of 1x level treenode       | number   | 30 (default)                                   |
| showCheckbox      | show check box?                         | bool     | true (default) | false                         |

---
## Bugs? Questions? Contributions?
Feel free to [open an issue](https://github.com/shunjizhan/React-Folder-Tree/issues), or create a pull request!
