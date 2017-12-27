# ignore-right-side-loader

**What's the use of it?**

Generally, our resources is processed by the common **loader**s which at **webpack**'s configuration files (such as webpack.config.js), but in some cases, there are some special resources which don't need to be processed by the common **loader**s, but use special **loader**s . This **loader** is used to ignore general **loader**s and providing raw resources.

**Which loaders can be ignored? **

well,the **"right side loader"** will be ignored.who are the **"right side loader"**?
for example,in ```require("a-loader!b-loader!ignore-right-side-loader!c-loader!d-loader!resource")
```,the **"c-loader","d-loader"** are the **"right side loader"** and they're will be ignored.so the above code equate to ```
require("a-loader!b-loader!resource")``` 

But I don't think anyone will use it like that. So please look at the following:

webpack.config.js
```
...
  rules:[{
    test:/\.txt$/,
    use:[
      "a-loader",
      "b-loader",
      "c-loader",
      "d-loader",
    ]
  }]
...
```  
test.js

```
  require("e-loader!f-loader!test.txt")
```  

now,the order of processing for "test.txt" is:
**test.txt => d-loader => c-loader => b-loader => a-loader => f-loader => e-loader => target**

but we only want that the "test.txt" be processed by **"e-loader"** and **"f-loader"**,ignore the common loaders (**"a-loader","b-loader","c-loader","d-loader"**),so we will use the **ignore-right-side-loader**:
```
  require("e-loader!f-loader!ignore-right-side-loader!test.txt")
```  
Now,the "test.txt" be processed only by **"f-loader" **and **"e-loader"**.

Well done!

