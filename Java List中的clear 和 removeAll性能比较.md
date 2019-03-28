---
title: Java List中的clear 和 removeAll性能比较
---



### 问题背景

有一个需求的场景就是，有个庞大的```List```，```List```里面所有的数据需要保存到```ES```中，最大的数据量可能达到10000k，数据的来源是从文本文件中获取来的，文本文件中的一行，作为一条记录，1K条数据向```ES```中存取一次。

计划每做一次写入的操作，对```List```进行一次清空的操作

```
    public void clear() {
        modCount++;

        // clear to let GC do its work
        for (int i = 0; i < size; i++)
            elementData[i] = null;

        size = 0;
    }
```

```
public boolean removeAll(Collection<?> c) {
        return batchRemove(c, false);
    }

    private boolean batchRemove(Collection<?> c, boolean complement) {
        final Object[] elementData = this.elementData;
        int r = 0, w = 0;
        boolean modified = false;
        try {
            for (; r < size; r++)
                if (c.contains(elementData[r]) == complement)
                    elementData[w++] = elementData[r];
        } finally {
            // Preserve behavioral compatibility with AbstractCollection,
            // even if c.contains() throws.
            if (r != size) {
                System.arraycopy(elementData, r,
                                 elementData, w,
                                 size - r);
                w += size - r;
            }
            if (w != size) {
                // clear to let GC do its work
                for (int i = w; i < size; i++)
                    elementData[i] = null;
                modCount += size - w;
                size = w;
                modified = true;
            }
        }
        return modified;
    }   

```