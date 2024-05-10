Execution Instructions
---------------------

```{bash}
docker build . -t fairlabs
docker run --rm -it -v $(pwd)/fairlabs_data.csv:/input.csv fairlabs
```