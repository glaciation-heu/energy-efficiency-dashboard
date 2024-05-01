# Energy efficiency dashboard

This repository contains a Grafana dashboard for displaying metrics (CPU, Memory, Storage) of a k8s pod and calculating the energy efficiency index of a node and workload.  

Energy index is a number greater than zero reflecting the energy efficiency of a node or workload. The higher the energy index, the greater the energy efficiency.  

- To measure the node energy index, run a standard workload on the node. To increase result accuracy, there should be no other workloads on the node.
- To measure the workload energy index, run the workload being tested. To increase result accuracy, resources allocated for the workload should be consistent across different workloads.

For comparing node energy index and workload energy index, the same metrics are used for the pod.

## Usage
1. Add the dashboard to Grafana
   - Go to the `Dashboards` section.
   - Click on the `New` button and choose `Import` from the dropdown menu.
   - Upload the file `dashboard.json` in the `Import dashboard from file` field or copy the content of the `dashboard.json` file into the `Import via panel json` field. Choose the folder and data source as `Prometheus`.
   - The `Energy efficiency` dashboard will be available in the list of dashboards in the `Dashboards` section.
2. Energy efficiency measurement
    - Setup resource limits in Helm Chart for workload. For example:
    ```json
        limits:
            cpu: "5"
            memory: "1024Mi"
            ephemeral-storage: "600Mi"
        requests:
            cpu: "3"
            memory: "800Mi"
            ephemeral-storage: "500Mi"
    ```
    - Deploy the workload (standard workload for measuring node energy index or the investigated workload for workload energy index) in one pod on one node.
    - Select the workload's pod in the dashboard filters.
    - The workload should run for some time to accumulate metric data. The longer the measurement, the more accurate the energy index.
    - The average value of the energy index will be available in the "Mean pod energy index" panel.

## Рабочие нагрузки для измерения энергоэффективности ноды
- [Stress workload](https://github.com/HIRO-MicroDataCenters-BV/workload-stress) 
    Stress workload launches a stress test on the node's server. It allows you to manage the intensity of the load on various components (CPU, Memory, Storage). This workload is useful for developing and testing methodology, but it is not applicable for measuring energy efficiency because the load is synthetic and does not correspond to the real workload exerted by GLACIATION workloads.
- [Simple workload](https://github.com/glaciation-heu/simple-workload) 
    Simple workload  simulates basic logic of the GLACIATION workload. During its operation, data is read from one Minio bucket, aggregated, and then written to another Minio bucket.
- [Complex workload](https://github.com/glaciation-heu/complex-workload) 
    Complex workload  is an enhanced version of the Simple workload. It employs distributed data processing. During the execution of this task, multiple instances concurrently read portions of data from one Minio bucket, perform computations on this data, and write the aggregated result to another Minio bucket.
- [Standard workload](https://github.com/glaciation-heu/standard-workload) 
    Standard workload  is most suitable for calculating the energy efficiency of GLACIATION nodes. It is based on the open-source NAS Parallel Benchmarks project. NAS Parallel Benchmarks is a set of benchmarks developed by NASA's Advanced Supercomputing Division to evaluate the performance of parallel supercomputers. These benchmarks include implementations of various real-world tasks like sorting, parallel computing, and solving linear equations, simulating real computational tasks when working with graphs in GLACIATION:
    •	Integer Sort, random memory access
    •	Embarrassingly Parallel
    •	Conjugate Gradient, irregular memory access and communication
    •	Multi-Grid on a sequence of meshes, long- and short-distance communication, memory intensive
    •	Discrete 3D fast Fourier Transform, all-to-all communication
    •	Block Tri-diagonal solver
    •	Scalar Penta-diagonal solver
    •	Lower-Upper Gauss-Seidel solver
    The load exerted by these benchmarks most accurately simulates the workload created by real GLACIATION workloads.

# Collaboration guidelines
HIRO uses and requires from its partners [GitFlow with Forks](https://hirodevops.notion.site/GitFlow-with-Forks-3b737784e4fc40eaa007f04aed49bb2e?pvs=4)

## License
[MIT](https://choosealicense.com/licenses/mit/)
