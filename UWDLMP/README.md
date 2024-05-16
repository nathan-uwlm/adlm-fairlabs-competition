Hosted version of the tool:
https://nathan-uwlm.github.io/adlm-fairlabs-competition/

Execution Instructions
----------------------

Build the docker image:

```{bash}
cd UWDLMP
docker build . -t fairlabs
cd ..
```

Generate the dashboard with a dataset of your choosing. You may select another dataset by substituting the path for fairlabs_data.csv. 
If the column structure differs, you can adapt it by definiting it in a substitute for the fairlab_input_dict.txt input file.

```
mkdir output
docker run --rm -it -v $(pwd)/fairlabs_data.csv:/input.csv -v $(pwd)/output:/output -v $(pwd)/UWDLMP/fairlab_input_dict.txt:/dict.txt fairlabs bash
```