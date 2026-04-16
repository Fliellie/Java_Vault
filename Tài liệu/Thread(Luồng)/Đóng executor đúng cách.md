```java
executor.shutdown(); 
try { if (!executor.awaitTermination(60, TimeUnit.SECONDS)) { executor.shutdownNow(); } } 
catch (InterruptedException e) { executor.shutdownNow(); }
```