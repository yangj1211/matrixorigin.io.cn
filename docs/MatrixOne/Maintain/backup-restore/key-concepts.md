# 备份与恢复相关概念

## 物理备份、快照备份与逻辑备份

### 物理备份

物理备份是将数据库文件直接复制到备份介质（如磁带、硬盘等）上的过程。此方式将数据库的所有物理数据块复制到备份介质，包括数据文件、控制文件和重做日志文件等。备份的数据是实际存储在磁盘上的二进制数据，恢复操作通常迅速。

### 快照备份

数据库快照备份为物理备份的一种形式，但区别于传统的物理备份，它通过捕捉数据库在特定时间点的只读静态视图来创建数据的即时副本。这种备份方式利用增量存储机制，仅记录自上一个快照以来发生变化的数据块，从而高效地使用存储空间。快照备份支持快速恢复，因为它们提供了数据库的完整一致性视图，适用于数据保护、报告生成、分析和其他需要数据一致性的场景。此外，它们通常依赖于底层存储系统的快照功能，能够在不影响数据库正常运行的情况下，为数据库提供一个安全的数据访问副本。

### 逻辑备份

逻辑备份是通过 SQL 语句备份数据库中的逻辑对象（如表、索引、存储过程等）。这种备份方式将逻辑对象的定义和数据导出至备份文件，但不涉及数据库文件的二进制数据。虽然恢复速度较慢，备份数据通常更易阅读和修改。

### 区别

数据物理备份、逻辑备份和快照备份是三种不同的数据保护策略：物理备份通过直接复制数据库的存储文件来创建数据库的一个完整副本，适用于快速恢复和大规模数据迁移；逻辑备份则导出数据库的逻辑结构，如 SQL 语句，以文本形式存储数据和结构，便于跨平台和版本的数据迁移；而快照备份是数据库在某一时刻的只读视图，利用增量存储技术记录变化，适用于快速恢复至特定时间点的状态，通常依赖于存储系统的支持。

## 全量备份、增量备份与差异备份

### 全量备份

全量备份将整个数据集备份至存储设备，包括所有文件和文件夹。尽管通常耗时长且需要大存储空间，但可完全还原数据，无需其他备份文件。

### 增量备份

增量备份仅备份最近更改或新增的文件或数据块。仅备份上次备份以来的变更，通常备份时间和存储空间较少。通常在定期全量备份之间执行，以确保备份数据保持最新。恢复数据需使用所有增量备份和最新全量备份。

### 差异备份

差异备份是备份自上次完整备份以来发生变化的数据，因此备份数据量较增量备份大，备份时间较长。恢复数据时，只需先恢复最近的完整备份，再恢复最近的差异备份即可。由于不依赖之前备份数据，恢复时间较短，备份和恢复过程相对简单。

### 全量备份、增量备份与差异备份的区别

- 全量备份提供完整数据备份，但需更多时间和存储空间。
- 增量备份适用数据变化较少环境，节省备份存储空间和时间，但恢复时间长，需依赖前备份数据。
- 差异备份适用数据变化较多环境，备份数据较大，恢复时间比增量备份短，且备份恢复过程相对简单。

## 恢复

### 物理恢复

物理恢复是利用物理备份进行数据库恢复。通常用于严重故障如硬盘故障、操作系统故障或文件系统故障。在物理恢复中，使用专业恢复工具或软件读取备份文件或存储介质中实际数据块，并尝试修复损坏块。

物理恢复优势在于快速还原数据库，因无需执行 SQL 语句或其他高层操作，直接处理数据块。此外，物理恢复能还原所有数据库对象，包括表、索引、存储过程等。

### 完全恢复与不完全恢复

- 完全恢复：应用备份集所有重做日志，将数据还原至备份中所有日志均已提交状态。
- 不完全恢复：应用备份集部分重做日志或增备，将数据库还原至备份重做日志包含的某一时间点。
