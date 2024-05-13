Execution Instructions
---------------------

```{bash}
mkdir output
docker build . -t fairlabs
docker run --rm -it -v $(pwd)/fairlabs_data.csv:/input.csv -v $(pwd)/output:/output fairlabs
```