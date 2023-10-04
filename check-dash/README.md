# Check dash

An action using [dash-license](https://github.com/eclipse/dash-licenses) to generate a license report and upload
results.

# How to use

You need an input file that Dash can understand, then give it as input like below

```
      - name: Dash license check
        uses: eclipse-kuksa/kuksa-actions/check-dash@2
        with:
          dashinput: ${{github.workspace}}/dash-databroker-deps
```


The uploaded result will be a zip file containing an `*.out` file with result similar to this:

```
[main] INFO Querying Eclipse Foundation for license data for 217 items.
[main] INFO Found 43 items.
[main] INFO Querying ClearlyDefined for license data for 174 items.
[main] INFO Found 174 items.
[main] INFO Vetted license information was found for all content. No further investigation is required.
```

More details are included in the `*.report` file. The input file is also uploaded.