```bash
docker  run -it --rm \
        --user "$(id -u):$(id -g)" \
        -v /path/to/bids_dataset:/bids_dataset \
        -v /path/to/output_dir:/output_dir \
        app_repository/app_name:version \
        /bids_dataset /output_dir analysis_level
```
