# parcels broken recipe

For some reason our recipe[^1] in Parcels has stopped working because it is (through the `uxarray` dependency) pulling in outdated versions of the `holoviews` and `hvplot` dependencies. This is despite the fact that newer versions `holoviews` and `hvplot` _are_ compatible with the recipe - leading me to believe that maybe this is an edge case that the SAT solver can't handle. 


> [!NOTE]  
> This can all be fixed by pinning `holoviews` above a certain version. I find this, however, surprising that we would need to either pin the transitive dependency `holoviews` or update the `uxarray` feedstock upstream to add these lower bounds.

## reproduction

- clone
- Install rattler (e.g., `pixi global install rattler-build`)

```bash
rattler-build --version     
rattler-build 0.59.0
```

- `rattler-build build --recipe recipe.yaml`

this creates the test environment with the following


<details><summary>Test environment deps</summary>
<p>

```
 │ │ ╭──────────────────────────────────┬──────────────┬─────────────────────────┬─────────────┬─────────────╮
 │ │ │ Package                          ┆ Version      ┆ Build                   ┆ Channel     ┆        Size │
 │ │ ╞══════════════════════════════════╪══════════════╪═════════════════════════╪═════════════╪═════════════╡
 │ │ │ _openmp_mutex                    ┆ 4.5          ┆ 7_kmp_llvm              ┆ conda-forge ┆    8.13 KiB │
 │ │ │ _python_abi3_support             ┆ 1.0          ┆ hd8ed1ab_2              ┆ conda-forge ┆    8.00 KiB │
 │ │ │ antimeridian                     ┆ 0.4.6        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   20.64 KiB │
 │ │ │ anyio                            ┆ 4.12.1       ┆ pyhcf101f3_0            ┆ conda-forge ┆  141.77 KiB │
 │ │ │ appnope                          ┆ 0.1.4        ┆ pyhd8ed1ab_1            ┆ conda-forge ┆    9.84 KiB │
 │ │ │ argon2-cffi                      ┆ 25.1.0       ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   18.28 KiB │
 │ │ │ argon2-cffi-bindings             ┆ 25.1.0       ┆ py314h0612a62_2         ┆ conda-forge ┆   33.42 KiB │
 │ │ │ arrow                            ┆ 1.4.0        ┆ pyhcf101f3_0            ┆ conda-forge ┆  111.19 KiB │
 │ │ │ asciitree                        ┆ 0.3.3        ┆ py_2                    ┆ conda-forge ┆    6.02 KiB │
 │ │ │ asttokens                        ┆ 3.0.1        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   28.12 KiB │
 │ │ │ async-lru                        ┆ 2.2.0        ┆ pyhcf101f3_0            ┆ conda-forge ┆   20.97 KiB │
 │ │ │ attrs                            ┆ 25.4.0       ┆ pyhcf101f3_1            ┆ conda-forge ┆   63.24 KiB │
 │ │ │ aws-c-auth                       ┆ 0.10.1       ┆ h8be066e_1              ┆ conda-forge ┆  114.08 KiB │
 │ │ │ aws-c-cal                        ┆ 0.9.13       ┆ h6ee9776_1              ┆ conda-forge ┆   44.17 KiB │
 │ │ │ aws-c-common                     ┆ 0.12.6       ┆ hc919400_0              ┆ conda-forge ┆  218.86 KiB │
 │ │ │ aws-c-compression                ┆ 0.3.2        ┆ h3e7f9b5_0              ┆ conda-forge ┆   20.97 KiB │
 │ │ │ aws-c-event-stream               ┆ 0.5.9        ┆ hd533cd8_2              ┆ conda-forge ┆   51.95 KiB │
 │ │ │ aws-c-http                       ┆ 0.10.11      ┆ ha1850f6_0              ┆ conda-forge ┆  168.39 KiB │
 │ │ │ aws-c-io                         ┆ 0.26.1       ┆ h4137820_2              ┆ conda-forge ┆  173.02 KiB │
 │ │ │ aws-c-mqtt                       ┆ 0.15.0       ┆ h38f3471_1              ┆ conda-forge ┆  152.92 KiB │
 │ │ │ aws-c-s3                         ┆ 0.11.5       ┆ hb540d77_4              ┆ conda-forge ┆  126.77 KiB │
 │ │ │ aws-c-sdkutils                   ┆ 0.2.4        ┆ h16f91aa_4              ┆ conda-forge ┆   52.18 KiB │
 │ │ │ aws-checksums                    ┆ 0.2.10       ┆ h3e7f9b5_0              ┆ conda-forge ┆   89.76 KiB │
 │ │ │ aws-crt-cpp                      ┆ 0.37.4       ┆ h61dc73a_1              ┆ conda-forge ┆  263.16 KiB │
 │ │ │ aws-sdk-cpp                      ┆ 1.11.747     ┆ h7519e1d_2              ┆ conda-forge ┆    3.11 MiB │
 │ │ │ azure-core-cpp                   ┆ 1.16.2       ┆ he5ae378_0              ┆ conda-forge ┆  284.11 KiB │
 │ │ │ azure-identity-cpp               ┆ 1.13.3       ┆ h810541e_1              ┆ conda-forge ┆  163.50 KiB │
 │ │ │ azure-storage-blobs-cpp          ┆ 12.16.0      ┆ hc57151b_1              ┆ conda-forge ┆  416.73 KiB │
 │ │ │ azure-storage-common-cpp         ┆ 12.12.0      ┆ he467506_1              ┆ conda-forge ┆  118.65 KiB │
 │ │ │ azure-storage-files-datalake-cpp ┆ 12.14.0      ┆ hf8a9d22_1              ┆ conda-forge ┆  193.51 KiB │
 │ │ │ babel                            ┆ 2.18.0       ┆ pyhcf101f3_1            ┆ conda-forge ┆    7.33 MiB │
 │ │ │ backports.zstd                   ┆ 1.3.0        ┆ py314h680f03e_0         ┆ conda-forge ┆    7.34 KiB │
 │ │ │ beautifulsoup4                   ┆ 4.14.3       ┆ pyha770c72_0            ┆ conda-forge ┆   88.28 KiB │
 │ │ │ bleach                           ┆ 6.3.0        ┆ pyhcf101f3_1            ┆ conda-forge ┆  138.68 KiB │
 │ │ │ bleach-with-css                  ┆ 6.3.0        ┆ hbca2aae_1              ┆ conda-forge ┆    4.31 KiB │
 │ │ │ blosc                            ┆ 1.21.6       ┆ h7dd00d9_1              ┆ conda-forge ┆   32.81 KiB │
 │ │ │ bokeh                            ┆ 3.9.0        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆    4.04 MiB │
 │ │ │ branca                           ┆ 0.8.2        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   29.47 KiB │
 │ │ │ brotli                           ┆ 1.2.0        ┆ h7d5ae5b_1              ┆ conda-forge ┆   19.76 KiB │
 │ │ │ brotli-bin                       ┆ 1.2.0        ┆ hc919400_1              ┆ conda-forge ┆   18.19 KiB │
 │ │ │ brotli-python                    ┆ 1.2.0        ┆ py314h3daef5d_1         ┆ conda-forge ┆  351.42 KiB │
 │ │ │ bzip2                            ┆ 1.0.8        ┆ hd037594_9              ┆ conda-forge ┆  121.91 KiB │
 │ │ │ c-ares                           ┆ 1.34.6       ┆ hc919400_0              ┆ conda-forge ┆  176.10 KiB │
 │ │ │ ca-certificates                  ┆ 2026.2.25    ┆ hbd8a1cb_0              ┆ conda-forge ┆  143.96 KiB │
 │ │ │ cached-property                  ┆ 1.5.2        ┆ hd8ed1ab_1              ┆ conda-forge ┆    4.04 KiB │
 │ │ │ cached_property                  ┆ 1.5.2        ┆ pyha770c72_1            ┆ conda-forge ┆   10.81 KiB │
 │ │ │ cartopy                          ┆ 0.25.0       ┆ py314ha3d490a_1         ┆ conda-forge ┆    1.46 MiB │
 │ │ │ certifi                          ┆ 2026.2.25    ┆ pyhd8ed1ab_0            ┆ conda-forge ┆  147.90 KiB │
 │ │ │ cf_xarray                        ┆ 0.10.11      ┆ pyhd8ed1ab_1            ┆ conda-forge ┆   68.67 KiB │
 │ │ │ cffi                             ┆ 2.0.0        ┆ py314h44086f9_1         ┆ conda-forge ┆  286.12 KiB │
 │ │ │ cftime                           ┆ 1.6.5        ┆ py314h2115a04_1         ┆ conda-forge ┆  388.11 KiB │
 │ │ │ charset-normalizer               ┆ 3.4.6        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   57.14 KiB │
 │ │ │ click                            ┆ 8.3.1        ┆ pyh8f84b5b_1            ┆ conda-forge ┆   95.39 KiB │
 │ │ │ cloudpickle                      ┆ 3.1.2        ┆ pyhcf101f3_1            ┆ conda-forge ┆   26.71 KiB │
 │ │ │ colorcet                         ┆ 3.1.0        ┆ pyhd8ed1ab_1            ┆ conda-forge ┆  169.87 KiB │
 │ │ │ comm                             ┆ 0.2.3        ┆ pyhe01879c_0            ┆ conda-forge ┆   14.35 KiB │
 │ │ │ contourpy                        ┆ 1.3.3        ┆ py314hf8a3a22_4         ┆ conda-forge ┆  283.60 KiB │
 │ │ │ cpython                          ┆ 3.14.3       ┆ py314hd8ed1ab_101       ┆ conda-forge ┆   48.90 KiB │
 │ │ │ cycler                           ┆ 0.12.1       ┆ pyhcf101f3_2            ┆ conda-forge ┆   14.43 KiB │
 │ │ │ cytoolz                          ┆ 1.1.0        ┆ py314h6c2aa35_2         ┆ conda-forge ┆  600.68 KiB │
 │ │ │ dask                             ┆ 2026.1.2     ┆ pyhcf101f3_1            ┆ conda-forge ┆   11.10 KiB │
 │ │ │ dask-core                        ┆ 2026.1.2     ┆ pyhcf101f3_0            ┆ conda-forge ┆    1.01 MiB │
 │ │ │ datashader                       ┆ 0.18.2       ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   16.44 MiB │
 │ │ │ debugpy                          ┆ 1.8.20       ┆ py314he609de1_0         ┆ conda-forge ┆    2.65 MiB │
 │ │ │ decorator                        ┆ 5.2.1        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   13.80 KiB │
 │ │ │ defusedxml                       ┆ 0.7.1        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   23.50 KiB │
 │ │ │ deprecated                       ┆ 1.3.1        ┆ pyhd8ed1ab_1            ┆ conda-forge ┆   15.52 KiB │
 │ │ │ distributed                      ┆ 2026.1.2     ┆ pyhcf101f3_1            ┆ conda-forge ┆  825.00 KiB │
 │ │ │ exceptiongroup                   ┆ 1.3.1        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   20.83 KiB │
 │ │ │ executing                        ┆ 2.2.1        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   30.03 KiB │
 │ │ │ fasteners                        ┆ 0.19         ┆ pyhd8ed1ab_1            ┆ conda-forge ┆   20.23 KiB │
 │ │ │ folium                           ┆ 0.20.0       ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   80.73 KiB │
 │ │ │ fonttools                        ┆ 4.62.0       ┆ pyh7db6752_0            ┆ conda-forge ┆  818.27 KiB │
 │ │ │ fqdn                             ┆ 1.5.1        ┆ pyhd8ed1ab_1            ┆ conda-forge ┆   16.31 KiB │
 │ │ │ freetype                         ┆ 2.14.2       ┆ hce30654_0              ┆ conda-forge ┆  169.71 KiB │
 │ │ │ freexl                           ┆ 2.0.0        ┆ h3ab3353_2              ┆ conda-forge ┆   52.13 KiB │
 │ │ │ fsspec                           ┆ 2026.2.0     ┆ pyhd8ed1ab_0            ┆ conda-forge ┆  145.27 KiB │
 │ │ │ future                           ┆ 1.0.0        ┆ pyhd8ed1ab_2            ┆ conda-forge ┆  356.02 KiB │
 │ │ │ gdal                             ┆ 3.12.2       ┆ py314h0ed7ee7_3         ┆ conda-forge ┆    1.76 MiB │
 │ │ │ geopandas                        ┆ 1.1.3        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆    8.56 KiB │
 │ │ │ geopandas-base                   ┆ 1.1.3        ┆ pyha770c72_0            ┆ conda-forge ┆  249.01 KiB │
 │ │ │ geos                             ┆ 3.14.1       ┆ h5afe852_0              ┆ conda-forge ┆    1.46 MiB │
 │ │ │ geoviews                         ┆ 1.6.6        ┆ py_1                    ┆ conda-forge ┆    4.50 KiB │
 │ │ │ geoviews-core                    ┆ 1.6.6        ┆ py_1                    ┆ conda-forge ┆  347.94 KiB │
 │ │ │ gflags                           ┆ 2.2.2        ┆ hf9b8971_1005           ┆ conda-forge ┆   80.17 KiB │
 │ │ │ giflib                           ┆ 5.2.2        ┆ h93a5062_0              ┆ conda-forge ┆   69.93 KiB │
 │ │ │ glog                             ┆ 0.7.1        ┆ heb240a5_0              ┆ conda-forge ┆  109.58 KiB │
 │ │ │ h11                              ┆ 0.16.0       ┆ pyhcf101f3_1            ┆ conda-forge ┆   38.15 KiB │
 │ │ │ h2                               ┆ 4.3.0        ┆ pyhcf101f3_0            ┆ conda-forge ┆   93.72 KiB │
 │ │ │ hdf4                             ┆ 4.2.15       ┆ h2ee6834_7              ┆ conda-forge ┆  744.39 KiB │
 │ │ │ hdf5                             ┆ 1.14.6       ┆ nompi_had3affe_106      ┆ conda-forge ┆    3.15 MiB │
 │ │ │ healpix                          ┆ 2025.1       ┆ py314hdcf55e8_2         ┆ conda-forge ┆   34.92 KiB │
 │ │ │ holoviews                        ┆ 1.12.7       ┆ py_0                    ┆ conda-forge ┆    3.26 MiB │
 │ │ │ hpack                            ┆ 4.1.0        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   30.01 KiB │
 │ │ │ httpcore                         ┆ 1.0.9        ┆ pyh29332c3_0            ┆ conda-forge ┆   48.32 KiB │
 │ │ │ httpx                            ┆ 0.28.1       ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   61.60 KiB │
 │ │ │ hvplot                           ┆ 0.8.1        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆    3.04 MiB │
 │ │ │ hyperframe                       ┆ 6.1.0        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   16.99 KiB │
 │ │ │ icu                              ┆ 78.2         ┆ hef89b57_0              ┆ conda-forge ┆   11.82 MiB │
 │ │ │ idna                             ┆ 3.11         ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   49.53 KiB │
 │ │ │ importlib-metadata               ┆ 8.7.0        ┆ pyhe01879c_1            ┆ conda-forge ┆   33.83 KiB │
 │ │ │ importlib_resources              ┆ 6.5.2        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   32.99 KiB │
 │ │ │ ipykernel                        ┆ 7.2.0        ┆ pyh5552912_1            ┆ conda-forge ┆  129.16 KiB │
 │ │ │ ipython                          ┆ 9.11.0       ┆ pyhecfbec7_0            ┆ conda-forge ┆  633.00 KiB │
 │ │ │ ipython_pygments_lexers          ┆ 1.1.1        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   13.67 KiB │
 │ │ │ ipywidgets                       ┆ 8.1.8        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆  111.70 KiB │
 │ │ │ isoduration                      ┆ 20.11.0      ┆ pyhd8ed1ab_1            ┆ conda-forge ┆   19.37 KiB │
 │ │ │ jedi                             ┆ 0.19.2       ┆ pyhd8ed1ab_1            ┆ conda-forge ┆  823.87 KiB │
 │ │ │ jinja2                           ┆ 3.1.6        ┆ pyhcf101f3_1            ┆ conda-forge ┆  117.86 KiB │
 │ │ │ joblib                           ┆ 1.5.3        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆  221.14 KiB │
 │ │ │ json-c                           ┆ 0.18         ┆ he4178ee_0              ┆ conda-forge ┆   71.99 KiB │
 │ │ │ json5                            ┆ 0.13.0       ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   33.22 KiB │
 │ │ │ jsonpointer                      ┆ 3.0.0        ┆ pyhcf101f3_3            ┆ conda-forge ┆   13.64 KiB │
 │ │ │ jsonschema                       ┆ 4.26.0       ┆ pyhcf101f3_0            ┆ conda-forge ┆   80.43 KiB │
 │ │ │ jsonschema-specifications        ┆ 2025.9.1     ┆ pyhcf101f3_0            ┆ conda-forge ┆   18.79 KiB │
 │ │ │ jsonschema-with-format-nongpl    ┆ 4.26.0       ┆ hcf101f3_0              ┆ conda-forge ┆    4.63 KiB │
 │ │ │ jupyter                          ┆ 1.1.1        ┆ pyhd8ed1ab_1            ┆ conda-forge ┆    8.68 KiB │
 │ │ │ jupyter-lsp                      ┆ 2.3.0        ┆ pyhcf101f3_0            ┆ conda-forge ┆   58.96 KiB │
 │ │ │ jupyter_client                   ┆ 8.8.0        ┆ pyhcf101f3_0            ┆ conda-forge ┆  110.14 KiB │
 │ │ │ jupyter_console                  ┆ 6.6.3        ┆ pyhd8ed1ab_1            ┆ conda-forge ┆   26.24 KiB │
 │ │ │ jupyter_core                     ┆ 5.9.1        ┆ pyhc90fa1f_0            ┆ conda-forge ┆   63.97 KiB │
 │ │ │ jupyter_events                   ┆ 0.12.0       ┆ pyhe01879c_0            ┆ conda-forge ┆   23.74 KiB │
 │ │ │ jupyter_server                   ┆ 2.17.0       ┆ pyhcf101f3_0            ┆ conda-forge ┆  338.96 KiB │
 │ │ │ jupyter_server_terminals         ┆ 0.5.4        ┆ pyhcf101f3_0            ┆ conda-forge ┆   21.54 KiB │
 │ │ │ jupyterlab                       ┆ 4.5.6        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆    7.86 MiB │
 │ │ │ jupyterlab_pygments              ┆ 0.3.0        ┆ pyhd8ed1ab_2            ┆ conda-forge ┆   18.27 KiB │
 │ │ │ jupyterlab_server                ┆ 2.28.0       ┆ pyhcf101f3_0            ┆ conda-forge ┆   50.41 KiB │
 │ │ │ jupyterlab_widgets               ┆ 3.0.16       ┆ pyhcf101f3_1            ┆ conda-forge ┆  211.70 KiB │
 │ │ │ kiwisolver                       ┆ 1.5.0        ┆ py314hf8a3a22_0         ┆ conda-forge ┆   67.66 KiB │
 │ │ │ krb5                             ┆ 1.22.2       ┆ h385eeb1_0              ┆ conda-forge ┆    1.11 MiB │
 │ │ │ lark                             ┆ 1.3.1        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   92.10 KiB │
 │ │ │ lcms2                            ┆ 2.18         ┆ hdfa7624_0              ┆ conda-forge ┆  206.79 KiB │
 │ │ │ lerc                             ┆ 4.1.0        ┆ h1eee2c3_0              ┆ conda-forge ┆  160.37 KiB │
 │ │ │ libabseil                        ┆ 20260107.1   ┆ cxx17_h2062a1b_0        ┆ conda-forge ┆    1.17 MiB │
 │ │ │ libaec                           ┆ 1.1.5        ┆ h8664d51_0              ┆ conda-forge ┆   29.68 KiB │
 │ │ │ libarchive                       ┆ 3.8.6        ┆ gpl_h6fbacd7_100        ┆ conda-forge ┆  773.01 KiB │
 │ │ │ libarrow                         ┆ 23.0.1       ┆ h2748d28_5_cpu          ┆ conda-forge ┆    4.05 MiB │
 │ │ │ libarrow-acero                   ┆ 23.0.1       ┆ hbf36091_5_cpu          ┆ conda-forge ┆  527.57 KiB │
 │ │ │ libarrow-compute                 ┆ 23.0.1       ┆ h4dbefc3_5_cpu          ┆ conda-forge ┆    2.15 MiB │
 │ │ │ libarrow-dataset                 ┆ 23.0.1       ┆ hbf36091_5_cpu          ┆ conda-forge ┆  523.73 KiB │
 │ │ │ libarrow-substrait               ┆ 23.0.1       ┆ h05be00f_5_cpu          ┆ conda-forge ┆  460.80 KiB │
 │ │ │ libblas                          ┆ 3.11.0       ┆ 5_h51639a9_openblas     ┆ conda-forge ┆   18.11 KiB │
 │ │ │ libbrotlicommon                  ┆ 1.2.0        ┆ hc919400_1              ┆ conda-forge ┆   77.58 KiB │
 │ │ │ libbrotlidec                     ┆ 1.2.0        ┆ hc919400_1              ┆ conda-forge ┆   28.76 KiB │
 │ │ │ libbrotlienc                     ┆ 1.2.0        ┆ hc919400_1              ┆ conda-forge ┆  283.94 KiB │
 │ │ │ libcblas                         ┆ 3.11.0       ┆ 5_hb0561ab_openblas     ┆ conda-forge ┆   18.11 KiB │
 │ │ │ libcrc32c                        ┆ 1.1.2        ┆ hbdafb3b_0              ┆ conda-forge ┆   18.33 KiB │
 │ │ │ libcurl                          ┆ 8.19.0       ┆ hd5a2499_0              ┆ conda-forge ┆  390.25 KiB │
 │ │ │ libcxx                           ┆ 22.1.1       ┆ h55c6f16_0              ┆ conda-forge ┆  556.92 KiB │
 │ │ │ libdeflate                       ┆ 1.25         ┆ hc11a715_0              ┆ conda-forge ┆   54.12 KiB │
 │ │ │ libedit                          ┆ 3.1.20250104 ┆ pl5321hafb1f1b_0        ┆ conda-forge ┆  105.17 KiB │
 │ │ │ libev                            ┆ 4.33         ┆ h93a5062_2              ┆ conda-forge ┆  104.94 KiB │
 │ │ │ libevent                         ┆ 2.1.12       ┆ h2757513_1              ┆ conda-forge ┆  359.54 KiB │
 │ │ │ libexpat                         ┆ 2.7.4        ┆ hf6b4638_0              ┆ conda-forge ┆   66.60 KiB │
 │ │ │ libffi                           ┆ 3.5.2        ┆ hcf2aa1b_0              ┆ conda-forge ┆   40.02 KiB │
 │ │ │ libfreetype                      ┆ 2.14.2       ┆ hce30654_0              ┆ conda-forge ┆    7.89 KiB │
 │ │ │ libfreetype6                     ┆ 2.14.2       ┆ hdfa99f5_0              ┆ conda-forge ┆  330.11 KiB │
 │ │ │ libgcc                           ┆ 15.2.0       ┆ hcbb3090_18             ┆ conda-forge ┆  392.55 KiB │
 │ │ │ libgdal-core                     ┆ 3.12.2       ┆ h38a4fdb_3              ┆ conda-forge ┆    9.44 MiB │
 │ │ │ libgfortran                      ┆ 15.2.0       ┆ h07b0088_18             ┆ conda-forge ┆  135.72 KiB │
 │ │ │ libgfortran5                     ┆ 15.2.0       ┆ hdae7583_18             ┆ conda-forge ┆  584.60 KiB │
 │ │ │ libgoogle-cloud                  ┆ 2.39.0       ┆ h2f60c08_1              ┆ conda-forge ┆  861.06 KiB │
 │ │ │ libgoogle-cloud-storage          ┆ 2.39.0       ┆ ha114238_1              ┆ conda-forge ┆  512.47 KiB │
 │ │ │ libgrpc                          ┆ 1.78.0       ┆ h3e3f78d_1              ┆ conda-forge ┆    4.64 MiB │
 │ │ │ libhwy                           ┆ 1.3.0        ┆ h48b13b8_1              ┆ conda-forge ┆  567.95 KiB │
 │ │ │ libiconv                         ┆ 1.18         ┆ h23cfdf5_2              ┆ conda-forge ┆  732.79 KiB │
 │ │ │ libjpeg-turbo                    ┆ 3.1.2        ┆ hc919400_0              ┆ conda-forge ┆  538.28 KiB │
 │ │ │ libjxl                           ┆ 0.11.2       ┆ h913acd8_0              ┆ conda-forge ┆ 1008.14 KiB │
 │ │ │ libkml                           ┆ 1.3.0        ┆ hc33e383_1022           ┆ conda-forge ┆  277.41 KiB │
 │ │ │ liblapack                        ┆ 3.11.0       ┆ 5_hd9741b5_openblas     ┆ conda-forge ┆   18.12 KiB │
 │ │ │ liblzma                          ┆ 5.8.2        ┆ h8088a28_0              ┆ conda-forge ┆   90.08 KiB │
 │ │ │ libmpdec                         ┆ 4.0.0        ┆ h84a0fba_1              ┆ conda-forge ┆   71.96 KiB │
 │ │ │ libnetcdf                        ┆ 4.10.0       ┆ nompi_h7a8d41e_100      ┆ conda-forge ┆  663.89 KiB │
 │ │ │ libnghttp2                       ┆ 1.67.0       ┆ hc438710_0              ┆ conda-forge ┆  561.97 KiB │
 │ │ │ libopenblas                      ┆ 0.3.30       ┆ openmp_ha158390_4       ┆ conda-forge ┆    4.09 MiB │
 │ │ │ libopentelemetry-cpp             ┆ 1.21.0       ┆ h08d5cc3_2              ┆ conda-forge ┆  547.04 KiB │
 │ │ │ libopentelemetry-cpp-headers     ┆ 1.21.0       ┆ hce30654_2              ┆ conda-forge ┆  355.57 KiB │
 │ │ │ libparquet                       ┆ 23.0.1       ┆ h7a13205_5_cpu          ┆ conda-forge ┆    1.02 MiB │
 │ │ │ libpng                           ┆ 1.6.55       ┆ h132b30e_0              ┆ conda-forge ┆  282.18 KiB │
 │ │ │ libprotobuf                      ┆ 6.33.5       ┆ h4a5acfd_0              ┆ conda-forge ┆    2.59 MiB │
 │ │ │ libre2-11                        ┆ 2025.11.05   ┆ h4c27e2a_1              ┆ conda-forge ┆  161.96 KiB │
 │ │ │ librttopo                        ┆ 1.1.0        ┆ ha909e78_20             ┆ conda-forge ┆  188.08 KiB │
 │ │ │ libsodium                        ┆ 1.0.21       ┆ h1a92334_3              ┆ conda-forge ┆  242.23 KiB │
 │ │ │ libspatialite                    ┆ 5.1.0        ┆ gpl_hc59e0ec_120        ┆ conda-forge ┆    2.59 MiB │
 │ │ │ libsqlite                        ┆ 3.52.0       ┆ h1ae2325_0              ┆ conda-forge ┆  896.89 KiB │
 │ │ │ libssh2                          ┆ 1.11.1       ┆ h1590b86_0              ┆ conda-forge ┆  272.65 KiB │
 │ │ │ libthrift                        ┆ 0.22.0       ┆ h14a376c_1              ┆ conda-forge ┆  315.78 KiB │
 │ │ │ libtiff                          ┆ 4.7.1        ┆ h4030677_1              ┆ conda-forge ┆  365.13 KiB │
 │ │ │ libutf8proc                      ┆ 2.11.3       ┆ h2431656_0              ┆ conda-forge ┆   85.86 KiB │
 │ │ │ libwebp-base                     ┆ 1.6.0        ┆ h07db88b_0              ┆ conda-forge ┆  288.06 KiB │
 │ │ │ libxcb                           ┆ 1.17.0       ┆ hdb1d25a_0              ┆ conda-forge ┆  316.07 KiB │
 │ │ │ libxml2                          ┆ 2.15.2       ┆ h8d039ee_0              ┆ conda-forge ┆   40.24 KiB │
 │ │ │ libxml2-16                       ┆ 2.15.2       ┆ h5ef1a60_0              ┆ conda-forge ┆  455.29 KiB │
 │ │ │ libxml2-devel                    ┆ 2.15.2       ┆ h8d039ee_0              ┆ conda-forge ┆   78.51 KiB │
 │ │ │ libzip                           ┆ 1.11.2       ┆ h1336266_0              ┆ conda-forge ┆  122.57 KiB │
 │ │ │ libzlib                          ┆ 1.3.1        ┆ h8359307_2              ┆ conda-forge ┆   45.35 KiB │
 │ │ │ llvm-openmp                      ┆ 22.1.0       ┆ hc7d1edf_0              ┆ conda-forge ┆  278.87 KiB │
 │ │ │ llvmlite                         ┆ 0.46.0       ┆ py314ha398f32_0         ┆ conda-forge ┆   23.20 MiB │
 │ │ │ locket                           ┆ 1.0.0        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆    8.06 KiB │
 │ │ │ lz4                              ┆ 4.4.5        ┆ py314h24f3bdd_1         ┆ conda-forge ┆  124.48 KiB │
 │ │ │ lz4-c                            ┆ 1.10.0       ┆ h286801f_1              ┆ conda-forge ┆  145.34 KiB │
 │ │ │ lzo                              ┆ 2.10         ┆ h925e9cb_1002           ┆ conda-forge ┆  149.17 KiB │
 │ │ │ mapclassify                      ┆ 2.10.0       ┆ pyhd8ed1ab_1            ┆ conda-forge ┆  791.83 KiB │
 │ │ │ markupsafe                       ┆ 3.0.3        ┆ py314h6e9b3f0_1         ┆ conda-forge ┆   26.62 KiB │
 │ │ │ matplotlib-base                  ┆ 3.10.8       ┆ py314hd63e3f0_0         ┆ conda-forge ┆    7.90 MiB │
 │ │ │ matplotlib-inline                ┆ 0.2.1        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   14.82 KiB │
 │ │ │ minizip                          ┆ 4.0.10       ┆ hff1a8ea_0              ┆ conda-forge ┆   76.57 KiB │
 │ │ │ mistune                          ┆ 3.2.0        ┆ pyhcf101f3_0            ┆ conda-forge ┆   72.51 KiB │
 │ │ │ msgpack-python                   ┆ 1.1.2        ┆ py314h784bc60_1         ┆ conda-forge ┆   90.22 KiB │
 │ │ │ multipledispatch                 ┆ 0.6.0        ┆ pyhd8ed1ab_1            ┆ conda-forge ┆   16.85 KiB │
 │ │ │ munkres                          ┆ 1.1.4        ┆ pyhd8ed1ab_1            ┆ conda-forge ┆   15.48 KiB │
 │ │ │ muparser                         ┆ 2.3.5        ┆ h11e0b38_0              ┆ conda-forge ┆  150.48 KiB │
 │ │ │ narwhals                         ┆ 2.18.0       ┆ pyhcf101f3_0            ┆ conda-forge ┆  273.85 KiB │
 │ │ │ nbclient                         ┆ 0.10.4       ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   27.81 KiB │
 │ │ │ nbconvert-core                   ┆ 7.17.0       ┆ pyhcf101f3_0            ┆ conda-forge ┆  197.54 KiB │
 │ │ │ nbformat                         ┆ 5.10.4       ┆ pyhd8ed1ab_1            ┆ conda-forge ┆   98.58 KiB │
 │ │ │ ncurses                          ┆ 6.5          ┆ h5e97a16_3              ┆ conda-forge ┆  778.35 KiB │
 │ │ │ nest-asyncio                     ┆ 1.6.0        ┆ pyhd8ed1ab_1            ┆ conda-forge ┆   11.27 KiB │
 │ │ │ netcdf4                          ┆ 1.7.4        ┆ nompi_py311h8d5b1ca_105 ┆ conda-forge ┆  973.16 KiB │
 │ │ │ networkx                         ┆ 3.6.1        ┆ pyhcf101f3_0            ┆ conda-forge ┆    1.51 MiB │
 │ │ │ nlohmann_json                    ┆ 3.12.0       ┆ h784d473_1              ┆ conda-forge ┆  134.37 KiB │
 │ │ │ notebook                         ┆ 7.5.5        ┆ pyhcf101f3_0            ┆ conda-forge ┆    9.65 MiB │
 │ │ │ notebook-shim                    ┆ 0.2.4        ┆ pyhd8ed1ab_1            ┆ conda-forge ┆   16.42 KiB │
 │ │ │ numba                            ┆ 0.64.0       ┆ py314hb38061f_0         ┆ conda-forge ┆    5.54 MiB │
 │ │ │ numcodecs                        ┆ 0.15.1       ┆ py314ha3d490a_1         ┆ conda-forge ┆  652.32 KiB │
 │ │ │ numpy                            ┆ 2.4.2        ┆ py314hae46ccb_1         ┆ conda-forge ┆    6.67 MiB │
 │ │ │ openjpeg                         ┆ 2.5.4        ┆ hd9e9057_0              ┆ conda-forge ┆  312.20 KiB │
 │ │ │ openssl                          ┆ 3.6.1        ┆ hd24854e_1              ┆ conda-forge ┆    2.96 MiB │
 │ │ │ orc                              ┆ 2.3.0        ┆ hd11884d_0              ┆ conda-forge ┆  535.33 KiB │
 │ │ │ overrides                        ┆ 7.7.0        ┆ pyhd8ed1ab_1            ┆ conda-forge ┆   29.43 KiB │
 │ │ │ packaging                        ┆ 26.0         ┆ pyhcf101f3_0            ┆ conda-forge ┆   70.32 KiB │
 │ │ │ pandas                           ┆ 3.0.1        ┆ py314h5e21a50_0         ┆ conda-forge ┆   13.67 MiB │
 │ │ │ pandocfilters                    ┆ 1.5.0        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   11.35 KiB │
 │ │ │ param                            ┆ 1.13.0       ┆ pyh1a96a4e_0            ┆ conda-forge ┆   78.08 KiB │
 │ │ │ parcels                          ┆ 0.1.0dev     ┆ pyh4616a5c_0            ┆ .tmphzPCkG  ┆   10.93 KiB │
 │ │ │ parso                            ┆ 0.8.6        ┆ pyhcf101f3_0            ┆ conda-forge ┆   80.36 KiB │
 │ │ │ partd                            ┆ 1.4.2        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   20.39 KiB │
 │ │ │ pcre2                            ┆ 10.47        ┆ h30297fc_0              ┆ conda-forge ┆  830.30 KiB │
 │ │ │ pexpect                          ┆ 4.9.0        ┆ pyhd8ed1ab_1            ┆ conda-forge ┆   52.31 KiB │
 │ │ │ pillow                           ┆ 12.1.1       ┆ py314hab283cf_0         ┆ conda-forge ┆  972.84 KiB │
 │ │ │ pip                              ┆ 26.0.1       ┆ pyh145f28c_0            ┆ conda-forge ┆    1.13 MiB │
 │ │ │ platformdirs                     ┆ 4.9.4        ┆ pyhcf101f3_0            ┆ conda-forge ┆   25.04 KiB │
 │ │ │ polars                           ┆ 1.38.1       ┆ pyh6a1acc5_0            ┆ conda-forge ┆  513.45 KiB │
 │ │ │ polars-runtime-32                ┆ 1.38.1       ┆ py310haaaf75b_0         ┆ conda-forge ┆   31.00 MiB │
 │ │ │ pooch                            ┆ 1.9.0        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   55.50 KiB │
 │ │ │ proj                             ┆ 9.8.0        ┆ hfb14a63_0              ┆ conda-forge ┆    3.03 MiB │
 │ │ │ prometheus-cpp                   ┆ 1.3.0        ┆ h0967b3e_0              ┆ conda-forge ┆  169.16 KiB │
 │ │ │ prometheus_client                ┆ 0.24.1       ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   55.31 KiB │
 │ │ │ prompt-toolkit                   ┆ 3.0.52       ┆ pyha770c72_0            ┆ conda-forge ┆  267.51 KiB │
 │ │ │ prompt_toolkit                   ┆ 3.0.52       ┆ hd8ed1ab_0              ┆ conda-forge ┆    7.04 KiB │
 │ │ │ psutil                           ┆ 7.2.2        ┆ py314ha14b1ff_0         ┆ conda-forge ┆  239.75 KiB │
 │ │ │ pthread-stubs                    ┆ 0.4          ┆ hd74edd7_1002           ┆ conda-forge ┆    8.18 KiB │
 │ │ │ ptyprocess                       ┆ 0.7.0        ┆ pyhd8ed1ab_1            ┆ conda-forge ┆   19.00 KiB │
 │ │ │ pure_eval                        ┆ 0.2.3        ┆ pyhd8ed1ab_1            ┆ conda-forge ┆   16.28 KiB │
 │ │ │ pyarrow                          ┆ 23.0.1       ┆ py314he55896b_0         ┆ conda-forge ┆   27.99 KiB │
 │ │ │ pyarrow-core                     ┆ 23.0.1       ┆ py314h109bba2_0_cpu     ┆ conda-forge ┆    3.76 MiB │
 │ │ │ pycparser                        ┆ 2.22         ┆ pyh29332c3_1            ┆ conda-forge ┆  107.52 KiB │
 │ │ │ pyct                             ┆ 0.6.0        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   21.62 KiB │
 │ │ │ pygments                         ┆ 2.19.2       ┆ pyhd8ed1ab_0            ┆ conda-forge ┆  868.44 KiB │
 │ │ │ pyobjc-core                      ┆ 12.1         ┆ py314h3a4d195_0         ┆ conda-forge ┆  472.04 KiB │
 │ │ │ pyobjc-framework-cocoa           ┆ 12.1         ┆ py314h36abed7_0         ┆ conda-forge ┆  366.01 KiB │
 │ │ │ pyogrio                          ┆ 0.12.1       ┆ py314h3da1bed_0         ┆ conda-forge ┆  585.82 KiB │
 │ │ │ pyparsing                        ┆ 3.3.2        ┆ pyhcf101f3_0            ┆ conda-forge ┆  108.29 KiB │
 │ │ │ pyproj                           ┆ 3.7.2        ┆ py314h986c384_4         ┆ conda-forge ┆  518.67 KiB │
 │ │ │ pyshp                            ┆ 3.0.3        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆  443.76 KiB │
 │ │ │ pysocks                          ┆ 1.7.1        ┆ pyha55dd90_7            ┆ conda-forge ┆   20.59 KiB │
 │ │ │ python                           ┆ 3.14.3       ┆ h4c637c5_101_cp314      ┆ conda-forge ┆   12.90 MiB │
 │ │ │ python-dateutil                  ┆ 2.9.0.post0  ┆ pyhe01879c_2            ┆ conda-forge ┆  227.84 KiB │
 │ │ │ python-fastjsonschema            ┆ 2.21.2       ┆ pyhe01879c_0            ┆ conda-forge ┆  238.89 KiB │
 │ │ │ python-gil                       ┆ 3.14.3       ┆ h4df99d1_101            ┆ conda-forge ┆   48.89 KiB │
 │ │ │ python-json-logger               ┆ 2.0.7        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   13.07 KiB │
 │ │ │ python-tzdata                    ┆ 2025.3       ┆ pyhd8ed1ab_0            ┆ conda-forge ┆  140.18 KiB │
 │ │ │ python_abi                       ┆ 3.14         ┆ 8_cp314                 ┆ conda-forge ┆    6.83 KiB │
 │ │ │ pyviz_comms                      ┆ 3.0.6        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   48.27 KiB │
 │ │ │ pyyaml                           ┆ 6.0.3        ┆ py314h6e9b3f0_1         ┆ conda-forge ┆  185.03 KiB │
 │ │ │ pyzmq                            ┆ 27.1.0       ┆ py312h022ad19_2         ┆ conda-forge ┆  187.15 KiB │
 │ │ │ qhull                            ┆ 2020.2       ┆ h420ef59_5              ┆ conda-forge ┆  504.27 KiB │
 │ │ │ re2                              ┆ 2025.11.05   ┆ ha480c28_1              ┆ conda-forge ┆   26.80 KiB │
 │ │ │ readline                         ┆ 8.3          ┆ h46df422_0              ┆ conda-forge ┆  306.57 KiB │
 │ │ │ referencing                      ┆ 0.37.0       ┆ pyhcf101f3_0            ┆ conda-forge ┆   50.57 KiB │
 │ │ │ requests                         ┆ 2.32.5       ┆ pyhcf101f3_1            ┆ conda-forge ┆   62.11 KiB │
 │ │ │ retrying                         ┆ 1.4.2        ┆ pyhe01879c_0            ┆ conda-forge ┆   20.42 KiB │
 │ │ │ rfc3339-validator                ┆ 0.1.4        ┆ pyhd8ed1ab_1            ┆ conda-forge ┆    9.97 KiB │
 │ │ │ rfc3986-validator                ┆ 0.1.1        ┆ pyh9f0ad1d_0            ┆ conda-forge ┆    7.63 KiB │
 │ │ │ rfc3987-syntax                   ┆ 1.1.0        ┆ pyhe01879c_1            ┆ conda-forge ┆   22.38 KiB │
 │ │ │ rpds-py                          ┆ 0.30.0       ┆ py314haad56a0_0         ┆ conda-forge ┆  342.75 KiB │
 │ │ │ scikit-learn                     ┆ 1.8.0        ┆ np2py314h15f0f0f_1      ┆ conda-forge ┆    8.95 MiB │
 │ │ │ scipy                            ┆ 1.17.1       ┆ py314hfc1f868_0         ┆ conda-forge ┆   13.34 MiB │
 │ │ │ send2trash                       ┆ 2.1.0        ┆ pyh5552912_1            ┆ conda-forge ┆   21.99 KiB │
 │ │ │ setuptools                       ┆ 82.0.1       ┆ pyh332efcf_0            ┆ conda-forge ┆  624.70 KiB │
 │ │ │ shapely                          ┆ 2.1.2        ┆ py314h277790e_2         ┆ conda-forge ┆  604.42 KiB │
 │ │ │ six                              ┆ 1.17.0       ┆ pyhe01879c_1            ┆ conda-forge ┆   18.02 KiB │
 │ │ │ snappy                           ┆ 1.2.2        ┆ hada39a4_1              ┆ conda-forge ┆   37.97 KiB │
 │ │ │ sniffio                          ┆ 1.3.1        ┆ pyhd8ed1ab_2            ┆ conda-forge ┆   15.33 KiB │
 │ │ │ sortedcontainers                 ┆ 2.4.0        ┆ pyhd8ed1ab_1            ┆ conda-forge ┆   27.99 KiB │
 │ │ │ soupsieve                        ┆ 2.8.3        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   37.29 KiB │
 │ │ │ spatialpandas                    ┆ 0.5.0        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   78.89 KiB │
 │ │ │ sqlite                           ┆ 3.52.0       ┆ h77b7338_0              ┆ conda-forge ┆  176.62 KiB │
 │ │ │ stack_data                       ┆ 0.6.3        ┆ pyhd8ed1ab_1            ┆ conda-forge ┆   26.36 KiB │
 │ │ │ tblib                            ┆ 3.2.2        ┆ pyhcf101f3_0            ┆ conda-forge ┆   18.94 KiB │
 │ │ │ terminado                        ┆ 0.18.1       ┆ pyhc90fa1f_1            ┆ conda-forge ┆   24.17 KiB │
 │ │ │ threadpoolctl                    ┆ 3.6.0        ┆ pyhecae5ae_0            ┆ conda-forge ┆   23.31 KiB │
 │ │ │ tinycss2                         ┆ 1.4.0        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   27.62 KiB │
 │ │ │ tk                               ┆ 8.6.13       ┆ h010d191_3              ┆ conda-forge ┆    2.98 MiB │
 │ │ │ tomli                            ┆ 2.4.0        ┆ pyhcf101f3_0            ┆ conda-forge ┆   20.95 KiB │
 │ │ │ toolz                            ┆ 1.1.0        ┆ pyhd8ed1ab_1            ┆ conda-forge ┆   52.71 KiB │
 │ │ │ tornado                          ┆ 6.5.4        ┆ py314h0612a62_0         ┆ conda-forge ┆  887.99 KiB │
 │ │ │ tqdm                             ┆ 4.67.3       ┆ pyh8f84b5b_0            ┆ conda-forge ┆   91.93 KiB │
 │ │ │ traitlets                        ┆ 5.14.3       ┆ pyhd8ed1ab_1            ┆ conda-forge ┆  107.47 KiB │
 │ │ │ typing-extensions                ┆ 4.15.0       ┆ h396c80c_0              ┆ conda-forge ┆   89.24 KiB │
 │ │ │ typing_extensions                ┆ 4.15.0       ┆ pyhcf101f3_0            ┆ conda-forge ┆   50.48 KiB │
 │ │ │ typing_utils                     ┆ 0.1.0        ┆ pyhd8ed1ab_1            ┆ conda-forge ┆   14.83 KiB │
 │ │ │ tzdata                           ┆ 2025c        ┆ hc9c84f9_1              ┆ conda-forge ┆  116.34 KiB │
 │ │ │ unicodedata2                     ┆ 17.0.1       ┆ py314h6c2aa35_0         ┆ conda-forge ┆  406.38 KiB │
 │ │ │ uri-template                     ┆ 1.3.0        ┆ pyhd8ed1ab_1            ┆ conda-forge ┆   23.43 KiB │
 │ │ │ uriparser                        ┆ 0.9.8        ┆ h00cdb27_0              ┆ conda-forge ┆   39.67 KiB │
 │ │ │ urllib3                          ┆ 2.6.3        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆  100.75 KiB │
 │ │ │ uxarray                          ┆ 2025.05.2    ┆ pyhd8ed1ab_0            ┆ conda-forge ┆  126.40 KiB │
 │ │ │ wcwidth                          ┆ 0.6.0        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   69.87 KiB │
 │ │ │ webcolors                        ┆ 25.10.0      ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   18.54 KiB │
 │ │ │ webencodings                     ┆ 0.5.1        ┆ pyhd8ed1ab_3            ┆ conda-forge ┆   15.13 KiB │
 │ │ │ websocket-client                 ┆ 1.9.0        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   59.95 KiB │
 │ │ │ widgetsnbextension               ┆ 4.0.15       ┆ pyhd8ed1ab_0            ┆ conda-forge ┆  868.35 KiB │
 │ │ │ wrapt                            ┆ 2.1.2        ┆ py314h6c2aa35_0         ┆ conda-forge ┆   83.64 KiB │
 │ │ │ xarray                           ┆ 2026.2.0     ┆ pyhcf101f3_0            ┆ conda-forge ┆  988.19 KiB │
 │ │ │ xerces-c                         ┆ 3.3.0        ┆ h25f632f_1              ┆ conda-forge ┆    1.22 MiB │
 │ │ │ xgcm                             ┆ 0.9.0        ┆ pyhd8ed1ab_0            ┆ conda-forge ┆    2.68 MiB │
 │ │ │ xorg-libxau                      ┆ 1.0.12       ┆ hc919400_1              ┆ conda-forge ┆   13.77 KiB │
 │ │ │ xorg-libxdmcp                    ┆ 1.1.5        ┆ hc919400_1              ┆ conda-forge ┆   18.71 KiB │
 │ │ │ xyzservices                      ┆ 2025.11.0    ┆ pyhd8ed1ab_0            ┆ conda-forge ┆   49.93 KiB │
 │ │ │ yaml                             ┆ 0.2.5        ┆ h925e9cb_3              ┆ conda-forge ┆   81.43 KiB │
 │ │ │ zarr                             ┆ 2.18.7       ┆ pyhd8ed1ab_0            ┆ conda-forge ┆  156.91 KiB │
 │ │ │ zeromq                           ┆ 4.3.5        ┆ h4818236_10             ┆ conda-forge ┆  239.50 KiB │
 │ │ │ zict                             ┆ 3.0.0        ┆ pyhd8ed1ab_1            ┆ conda-forge ┆   35.49 KiB │
 │ │ │ zipp                             ┆ 3.23.0       ┆ pyhcf101f3_1            ┆ conda-forge ┆   23.63 KiB │
 │ │ │ zlib                             ┆ 1.3.1        ┆ h8359307_2              ┆ conda-forge ┆   75.79 KiB │
 │ │ │ zlib-ng                          ┆ 2.3.3        ┆ hed4e4f5_1              ┆ conda-forge ┆   92.16 KiB │
 │ │ │ zstd                             ┆ 1.5.7        ┆ hbf9d68e_6              ┆ conda-forge ┆  423.25 KiB │
 │ │ ╰──────────────────────────────────┴──────────────┴─────────────────────────┴─────────────┴─────────────╯
```

</p>
</details> 

Then I get the error:

```
 │ │ holoviews 1.12.7 is not supported on this platform
 │ │ × error Script failed with status 1
 │ │ × error 
 │ │ × error Script execution failed.
 │ │ × error 
 │ │ × error   Work directory: /var/folders/j5/x0w7f5sn7gg25__6pg48j82c0000gn/T/.tmpLFhRyW/parcels-0.1.0dev-pyh4616a5c_0
 │ │ × error   Prefix: /Users/Hodgs004/coding/repos/tmp-parcels-broken-rattler/output/test/test_parcelsXwQk0s/test_env
 │ │ × error   Build prefix: None
 │ │ × error 
 │ │ × error To run the script manually, use the following command:
 │ │ × error 
 │ │ × error   cd "/var/folders/j5/x0w7f5sn7gg25__6pg48j82c0000gn/T/.tmpLFhRyW/parcels-0.1.0dev-pyh4616a5c_0" && ./conda_build.sh
 │ │ × error 
 │ │ × error To run commands interactively in the build environment:
 │ │ × error 
 │ │ × error   cd "/var/folders/j5/x0w7f5sn7gg25__6pg48j82c0000gn/T/.tmpLFhRyW/parcels-0.1.0dev-pyh4616a5c_0" && source build_env.sh
 │ │

```
I have also gotten errors where it seems to have continued anyway trying to run with holoviews 1.12.7 (Mar 2020) then crashing as its trying to import language features that have since been deprecated in newer Pythons. I can't reproduce these errors. Also outdated hvplot is being pulled in `1.8.1`.

My machine is `osx-arm64`.


[^1]: This recipe is not yet active (it is being used to build alpha versions for Parcels) so this doesn't correspond with an active feedstock .