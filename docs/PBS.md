# PBS

- PBS(Portable Batch System)最初由NASA的Ames研究中心开发，主要为了提供一个能满足异构计算网络需要的软件包，用于灵活的批处理，特别是满足高性能计算的需要，如集群系统、超级计算机和大规模并行系统。PBS的主要特点有：代码开放，免费获取；支持批处理、交互式作业和串行、多种并行作业，如MPI、 PVM、HPF、MPL；PBS是功能最为齐全, 历史最悠久, 支持最广泛的本地集群调度器之一.
- PBS的目前包括openPBS, PBS Pro和Torque三个主要分支. 其中OpenPBS是最早的PBS系统, 目前已经没有太多后续开发, PBS pro是PBS的商业版本, 功能最为丰富. Torque是Clustering公司接过了OpenPBS, 并给与后续支持的一个开源版本.

## 目录

# PBS命令

在shell等终端中可以使用的命令有：

1. **`qsub` 命令：用于提交作业脚本**
    
    **命令格式：**
    
    ```bash
    qsub [-a date_time]
    [-e path] [-I] [-l resource_list]
    [-M user_list] [-N name]
    [-S path_list] [-u user_list]
    [-W additional_attributes]
    ```
    
    ### 1. `-a date_time`
    
    - 含义：指定作业何时开始执行。
    - 格式：`[[CC]YY]MMDDhhmm[.SS]`
        - `CC`：世纪
        - `YY`：年份
        - `MM`：月份
        - `DD`：日期
        - `hh`：小时（24小时制）
        - `mm`：分钟
        - `SS`：秒（可选）
    - 用途：可以设置作业延迟启动。例如，`qsub -a 202409151230 my_job.sh` 会在2024年9月15日12:30运行作业。
    
    ### 2. `-e path`
    
    - 含义：指定作业的标准错误输出路径。
    - 用途：用于将作业运行过程中产生的错误信息输出到指定文件。如果不指定，默认错误输出会保存在作业运行目录中。例如，`qsub -e /path/to/error_file my_job.sh` 将错误输出保存到 `/path/to/error_file`。
    
    ### 3. `-I`
    
    - 含义：交互模式。
    - 用途：允许用户提交交互式作业。使用该选项时，用户将直接进入作业节点，执行交互式命令。交互作业通常适用于调试和开发。
    
    ### 4. `-l resource_list`
    
    - 含义：指定作业所需的资源列表。
    - 格式：`resource_name=value`
    - 用途：资源列表定义作业所需的计算资源。例如，指定CPU核心数、内存大小、节点数量等资源。常见资源有：
        - `nodes`：需要的节点数。
        - `mem`：所需内存。
        - `walltime`：预计运行时间。例如，`qsub -l nodes=2:ppn=4,walltime=10:00:00 my_job.sh` 表示作业需要2个节点，每个节点4个核心，预计运行时间为10小时。
    
    ### 5. `-M user_list`
    
    - 含义：指定作业通知的邮件地址列表。
    - 用途：当作业的状态发生变化（例如作业开始、完成、失败等）时，PBS会发送通知到指定的邮箱。例如，`qsub -M user@example.com my_job.sh` 会在作业完成或错误时发送邮件通知到 `user@example.com`。
    
    ### 6. `-N name`
    
    - 含义：指定作业名称。
    - 用途：为作业指定一个可识别的名字。作业名称可以帮助用户更容易地在队列中识别特定作业。例如，`qsub -N my_job my_job.sh` 将作业命名为 `my_job`。
    
    ### 7. `-S path_list`
    
    - 含义：指定用于执行作业的shell路径。
    - 用途：PBS将使用指定的shell来解释作业脚本。常用shell包括 `/bin/bash`, `/bin/sh` 等。例如，`qsub -S /bin/bash my_job.sh` 将使用bash shell来执行作业。
    
    ### 8. `-u user_list`
    
    - 含义：指定作业的用户列表。
    - 用途：用于为其他用户提交作业（需要管理员权限）。通常用于共享作业或者在多用户环境中为特定用户运行作业。例如，`qsub -u user1,user2 my_job.sh` 可以为 `user1` 和 `user2` 提交作业。
    
    ### 9. `-W additional_attributes`
    
    - 含义：设置附加的作业属性。
    - 用途：此参数允许设置其他高级作业属性。常见的附加属性包括：
        - `depend=dependency_list`：设置作业依赖。例如，某个作业只有在另一个作业完成后才能运行。`dependency_list` 可以是依赖的作业ID列表。
        - `group_list`：指定作业所属的组。
        
        **例如，`qsub -W depend=afterok:12345 my_job.sh` 表示只有当作业ID为12345的作业成功完成后，当前作业才会运行。**
        
2. **`qstat` 命令：用于查询作业状态信息**
    
           **命令格式：**
    
    ```bash
    qstat [-f][-a][-i] [-n][-s] [-R] [-Q][-q][-B][-u]
    ```
    
    1. **-f jobid 列出指定作业的信息**
    2. **-a 列出系统所有作业**
    3. **-i 显示等待执行的作业信息**
    4. **-n 列出分配给此作业的结点**
    5. **-s 显示简短的作业状态信息**
    6. **-R 显示资源使用报告**
    7. **-Q 显示队列的统计信息**
    8. **-q 显示可用队列信息**
    9. **-u userid 列出指定用户的所有作业**
    10. **-B 列出PBS Server信息**
3. **`qdel` 命令：用于删除已提交的作业**
    
    命令格式：
    
    ```bash
    qdel [-W 间隔时间] 作业号
    ```
    
4. **`qmgr` 命令：用于队列管理 （通常只有具有管理员权限才可以使用，用户用不上）**
    - qmgr详细用法
        
        `qmgr` 是 PBS（Portable Batch System）和 Torque 中的命令行工具，用于管理和配置作业调度系统的设置。通过 `qmgr`，管理员可以管理服务器、队列和节点的配置。通常只有具有管理员权限的用户才可以使用 `qmgr` 进行系统级操作。
        
        以下是 `qmgr` 的常见用法和操作：
        
        ### 1. 进入 `qmgr` 交互模式
        
        首先，打开终端并输入 `qmgr` 进入交互模式：
        
        ```bash
        qmgr
        
        ```
        
        进入交互模式后，提示符会变成 `Qmgr:`，表示你可以开始输入管理命令。
        
        ### 2. 直接使用 `qmgr` 执行单个命令
        
        你也可以直接在终端使用 `qmgr` 执行单个命令，而不进入交互模式。例如：
        
        ```bash
        qmgr -c "list server"
        
        ```
        
        ### 3. 常见命令用法
        
        ### 3.1 查看配置信息
        
        - **查看服务器配置**：
            
            ```bash
            qmgr -c "list server"
            
            ```
            
            该命令会列出PBS服务器的当前配置。
            
        - **查看队列配置**：
            
            ```bash
            qmgr -c "list queue queue_name"
            
            ```
            
            例如：
            
            ```bash
            qmgr -c "list queue workq"
            
            ```
            
            该命令会显示名为 `workq` 的队列的详细配置信息。
            
        - **查看节点配置**：
            
            ```bash
            qmgr -c "list node node_name"
            
            ```
            
            列出指定节点的配置信息。如果想查看所有节点，可以用：
            
            ```bash
            qmgr -c "list node"
            
            ```
            
        
        ### 3.2 创建和删除队列
        
        - **创建新队列**：
            
            ```bash
            qmgr -c "create queue queue_name"
            
            ```
            
            例如：
            
            ```bash
            qmgr -c "create queue new_queue"
            
            ```
            
            该命令会创建名为 `new_queue` 的新队列。
            
        - **删除队列**：
            
            ```bash
            qmgr -c "delete queue queue_name"
            
            ```
            
            例如：
            
            ```bash
            qmgr -c "delete queue old_queue"
            
            ```
            
            删除名为 `old_queue` 的队列。
            
        
        ### 3.3 修改队列或服务器属性
        
        - **设置队列属性**：
            
            ```bash
            qmgr -c "set queue queue_name attribute=value"
            
            ```
            
            例如：
            
            ```bash
            qmgr -c "set queue workq max_running=10"
            
            ```
            
            该命令将 `workq` 队列的 `max_running` 属性设置为 `10`，即该队列中同时允许运行的作业数。
            
        - **设置服务器属性**：
            
            ```bash
            qmgr -c "set server attribute=value"
            
            ```
            
            例如：
            
            ```bash
            qmgr -c "set server scheduling=true"
            
            ```
            
            该命令开启作业调度功能。
            
        
        ### 3.4 管理节点
        
        - **添加新节点**：
            
            ```bash
            qmgr -c "create node node_name"
            
            ```
            
            例如：
            
            ```bash
            qmgr -c "create node node01"
            
            ```
            
            创建一个名为 `node01` 的节点。
            
        - **删除节点**：
            
            ```bash
            qmgr -c "delete node node_name"
            
            ```
            
            例如：
            
            ```bash
            qmgr -c "delete node node01"
            
            ```
            
        - **设置节点属性**：
            
            ```bash
            qmgr -c "set node node_name attribute=value"
            
            ```
            
            例如：
            
            ```bash
            qmgr -c "set node node01 state=offline"
            
            ```
            
            将节点 `node01` 设置为离线状态。
            
        
        ### 3.5 使配置生效
        
        - **启用队列**：
            
            ```bash
            qmgr -c "set queue queue_name enabled=true"
            
            ```
            
            例如：
            
            ```bash
            qmgr -c "set queue new_queue enabled=true"
            
            ```
            
            启用 `new_queue` 队列。
            
        - **启动队列**：
            
            ```bash
            qmgr -c "set queue queue_name started=true"
            
            ```
            
            例如：
            
            ```bash
            qmgr -c "set queue new_queue started=true"
            
            ```
            
            这表示 `new_queue` 队列已准备好接受作业。
            
        
        ### 3.6 停止和启用调度
        
        - **停止调度器**：
            
            ```bash
            qmgr -c "set server scheduling=false"
            
            ```
            
            暂停调度器，作业将不会调度运行。
            
        - **启用调度器**：
            
            ```bash
            qmgr -c "set server scheduling=true"
            
            ```
            
            启动调度器，作业将正常调度。
            
        
        ### 4. 退出 `qmgr`
        
        如果你进入了交互模式，输入以下命令即可退出：
        
        ```bash
        exit
        
        ```
        
        ### 示例
        
        ### 查看所有队列的详细信息：
        
        ```bash
        qmgr -c "list queue"
        
        ```
        
        ### 创建并配置新队列：
        
        ```bash
        qmgr -c "create queue batch"
        qmgr -c "set queue batch queue_type=execution"
        qmgr -c "set queue batch enabled=true"
        qmgr -c "set queue batch started=true"
        
        ```
        
        ### 设置最大作业数：
        
        ```bash
        qmgr -c "set server max_running=100"
        
        ```
        
        ### 总结
        
        `qmgr` 是一个非常强大的管理工具，可以用于管理PBS服务器、队列和节点的各项配置。作为管理员，通过 `qmgr` 可以动态调整调度系统的行为和资源分配策略，确保高效地管理计算资源。
        

# PBS脚本

```bash
#!/bin/bash
#PBS -N my_job                # 作业名称
#PBS -l nodes=2:ppn=4         # 资源请求，2个节点，每节点4个核心
#PBS -l walltime=10:00:00     # 最大运行时间，格式为 hh:mm:ss
#PBS -l mem=8gb               # 请求作业所需的内存
#PBS -q batch                 # 指定队列
#PBS -o /path/to/output_file  # 标准输出文件路径
#PBS -e /path/to/error_file   # 错误输出文件路径
#PBS -M user@example.com      # 邮件通知地址
#PBS -m abe                   # 邮件通知选项：a=作业开始时，b=作业结束时，e=作业失败时
#PBS -j                       # 表示系统输出，
															# 如果是oe，则标准错误输出(stderr)和标准输出(stdout)合并为stdout，
	                            # 如果是eo，则合并为stderr
		                          # 如果没有设定或设定为n，则stderr和stdout分开。
#PBS -P priority              # 设置作业的优先级，值在-1024到+1024之间。数值越大，优先级越高。
#PBS -W depend=afterok:12345  # 设置作业依赖其他作业的完成情况。例如，某作业只有在另一个作业成功完成后才能开始。

# 切换到作业的工作目录（PBS默认启动时在用户的主目录）
cd $PBS_O_WORKDIR

# 加载需要的模块或环境
module load python/3.8

# 执行程序或脚本
python my_script.py
```