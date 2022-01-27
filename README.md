# Minimal GPF instance for HG19

## Install GPF system

### Install GPF from conda


### Install GPF from source

1. Clone the source code repository from https://github.com/iossifovlab/gpf/ 
    and switch to `gpf` directory

2. Setup `gpf` python environment using `conda`:

    ```bash
    conda env create --name gpf --file ./environment.yml
    conda env update --name gpf --file ./dev-environment.yml
    ```

3. Activate the `gpf` environment:
    ```bash
    conda activate gpf
    ```

## Prepare GPF instance config

1. Clone `data-hg19-empty` instance repository from 
    https://github.com/iossifovlab/data-hg19-empty and switch to 
    `data-hg19-empty` directory

2. Review the content of `gpf_instance.yaml` file. It specifies the minimal 
    configuration needed for a GPF instance:

    * reference genome
    * gene models

3. Consider using cache for Genomic Resource Repository. By default the GPF system 
    uses Genomic Resources Repository (GRR) located at 
    https://www.iossifovlab.com/distribution/public/genomic-resources-repository/ 
    without caching resources. You can change this adding following snippet into 
    your `gpf_instance.yaml` file:

    ```yaml
    grr:
        id: "%(instance_id)s"
        type: "url"
        url: "https://www.iossifovlab.com/distribution/public/genomic-resources-repository/"
        cache_dir: "%($HOME)s/grrCache"
    ```
    The `cache_dir` should point to a folder in your local filesystem that is suitable for storing large files.

4. Consider using `impala` genotype storage. By default the GPF system uses 
    filesystem genotype storage that has limited capabilities. You can switch to 
    using `impala` genotype storage by adding and adapting the following snippet 
    into your `gpf_instance.yaml` file:

    ```yaml
    genotype_storage:
        default: genotype_impala

    storage:
        genotype_impala:
            dir: "%($DAE_DB_DIR)s/work/"
            hdfs:
                base_dir: /user/%(instance_id)s/studies
                host: localhost
                port: 8020
                replication: 1
            impala:
                db: "%(instance_id)s"
                hosts:
                - localhost
                pool_size: 3
                port: 21050
            storage_type: impala
    ```

5. When ready with GPF instance configuration adjustments define environment 
    variable `DAE_DB_DIR` to point to the `data-hg19-empty` directory:

    ```bash
    export DAE_DB_DIR=<path to data-hg19-empty directory>
    ```
