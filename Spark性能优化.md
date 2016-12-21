###`性能 ＝（计算时间＋移动时间＋调度时间）／并行度`<br/>
==============================
####1,使用reduceByKey比groupByKey效率更高
![ReduceByKey](https://github.com/hhua161031/Spark/blob/master/image/reduceByKey.png)<br/>
![GroupByKey](https://github.com/hhua161031/Spark/blob/master/image/GroupByKey.png)<br>

==============================================
####2,repartition and coalesce
&emsp;&emsp;两个方法使用都是对RDD的重新分片，区别在于在分片时候reparation对shuffle是true，
而coalesce 是false,所以如果是rdd分区前后差别太大，过分激烈需要的进行shuffle操作，例如从1000分为1，则要使用shuffle使用reparation。<br/>

-------------------------------------------
`def coalesce(numPartitions: Int, shuffle: Boolean = false)(implicit ord: Ordering[T] = null)`<br/>
`  : RDD[T] = withScope {`<br/>
` if (shuffle) {`<br/>
`    /** Distributes elements evenly across output partitions, starting from a random partition. */`<br/>
`  val distributePartition = (index: Int, items: Iterator[T]) => {`<br>
`     var position = (new Random(index)).nextInt(numPartitions)`<br>
`   items.map { t =>`<br>
`        // Note that the hash code of the key will just be the key itself. The HashPartitioner`<br>
`       // will mod it with the number of total partitions.`<br>
`       position = position + 1`<br>
`        (position, t)`<br>
`      }`<br>
`   } : Iterator[(Int, T)]`<br>
`  // include a shuffle step so that our upstream tasks are still distributed`<br>
`  new CoalescedRDD(`<br>
`     new ShuffledRDD[Int, T, T](mapPartitionsWithIndex(distributePartition),`<br>
`    new HashPartitioner(numPartitions)),`<br>
`     numPartitions).values`<br>
`  } else {`<br>
`    new CoalescedRDD(this, numPartitions)`<br>
`  }`<br>
`}`<br>
     
-------------------------------------------
`  def repartition(numPartitions: Int)(implicit ord: Ordering[T] = null): RDD[T] = withScope {`<br>
`    coalesce(numPartitions, shuffle = true)`<br>
`  }`<br>

==============================================
####3,shuffle block size limitation<br>
![shuffleblock](https://github.com/hhua161031/Spark/blob/master/image/shuffleblock.png)<br>
&emsp;&emsp;Spark 使用一个叫 ByteBuffer 的数据结构来作为 shuffle 数据的缓存，但这个 ByteBuffer 默认分配的内存是 2g，
所以一旦 shuffle 的数据超过 2g 的时候，shuflle 过程会出错。影响 shuffle 数据大小的因素有以下常见的几个：
partition 的数量，partition 越多，分布到每个 partition 上的数据越少，越不容易导致 shuffle 数据过大;
数据分布不均匀，一般是 groupByKey 后，存在某几个 key 包含的数据过大，导致该 key 所在的 partition 上数据过大，
有可能触发后期 shuflle block 大于 2g;
一般解决这类办法都是增加 partition 的数量，Top 5 Mistakes When Writing Spark Applications 这里说可以预计让每个 partition 上的数据为 128MB 左右
，仅供参考，还是需要具体场景具体分析，这里只把原理讲清楚就行了，并没有一个完美的规范。<br>
* sc.textfile 时指定一个比较大的 partition number
* spark.sql.shuffle.partitions
* rdd.repartition
* rdd.coalesce


