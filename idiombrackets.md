

Idiom brackets and the applicative pattern: 

If you ever see something of the form `f <$> x1 <*> x2....<*> xn`, this is equivalent to the following: 

```
do 
  x1' <- x1 
  x2' <- x2 
  ...
  xn' <- xn 
  return $ f x1' x2'...xn' 
```
for some n. 

For instance, say you want to add all possibilities of values together, say, to simulate rolling 2d6. 

Then, you might write 

```
do 
  d1 <- [1..6] 
  d2 <- [1..6] 
  return $ d1 + d2 
```

However, you may notice that each "extraction" (the left hand side of the arrow) never matters on any previous values of the computation, so it would not change the program's semantics if instead we write 

```
do 
  d2 <- [1..6]
  d1 <- [1..6]
  return $ d1 + d2 
```

This is in contrast to a different program, still computing over die rolls: 
```
do 
  d1 <- [1..6] 
  d2 <- [1..d1] 
  return $ d1 + d2 
```

In this case, you can't write 
```
do 
  d2 <- [1..d1] 
  d1 <- [1..6] 
  return $ d1 + d2 
```
legally even. 

So, if you wish to write the legal applicative program (2d6), then instead of the cumbersome do statement, or its desugaring (`[1..6] >>= (\d1 -> [1..6] >>= (\d2 -> return $ d1 + d2)`), we can use the aforementioned applicative syntax: `(+) <$> [1..6] <*> [1..6]`. 
