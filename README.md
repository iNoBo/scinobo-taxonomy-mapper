# scinobo-taxonomy-mapper
This repository is used to map other taxonomies/keywords/phrases to the SciNoBo FoS taxonomy. 

Given a phrase or a sentence related to a Field of Science, the tool outputs the most similar 
L1/L2/L3/L4/L5/L6 FoS paths.

The tool can be used for identifying relevant FoS to a use case. Additionally, providing 
sentences like: "breast cancer in artificial intelligence" it will result in FoS paths relevant to 
"breast cancer" & "artificial intelligence".

The tool uses `instructor-xl` embeddings and `dense retrieval`.

## Info

The server provides one functionality:

1. **Infer Mapper**: This functionality allows you to infer the most similar L1/L2/L3/L4/L5/L6 FoS paths given a user query. The endpoint for this functionality is `/infer_mapper`. 

## Docker Build and Run

To build the Docker image, use the following command:

```bash
docker build -t scinobo-fos-mapper .
```

To run the Docker container, create an environment file with the following values:
- ES_PASSWD: the password to the elastic installation
- CA_CERTS_PATH: the path to the certification for elastic
- DEVICE: "cuda" or "cpu"
- ES_HOST: the ip of the elastic installation
- ES_PORT: the port of the elastic installation

### Server:
```bash
docker run --env-file <path to the env file> -i -d --name scinobo-fos-mapper-docker --rm --network host --gpus all -p 1990:1990 -v <path to the certs in host>:/certs/ scinobo-fos-mapper uvicorn fos_mapper.server.api:app --host 0.0.0.0 --port 1990
```

This will serve the uvicorn server of `api.py` which supports the `/infer_mapper` endpoint.