

Haskell myths: 

Do statements need not end in return, if you don't want. However, you may convert any do statement into one that ends in return. 

For instance, consider the following do statement. 

```
do 
  msg <- getLine
  putStrLn msg 
```

This is equivalent to 

```
do 
  msg <- getLine 
  result <- putStrLn msg 
  return result 
```

(This also happens to be equal to `do { msg <- getLine; putStrLn msg; return ()}`, but this is only because `putStrLn :: String -> IO ()`.) 



