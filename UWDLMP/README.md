Execution Instructions
---------------------

```{bash}
cd UWDLMP
docker build . -t fairlabs
cd ..
mkdir output
docker run --rm -it -v $(pwd)/fairlabs_data.csv:/input.csv -v $(pwd)/output:/output -v $(pwd)/UWDLMP/fairlab_input_dict.txt:/dict.txt fairlabs bash
```