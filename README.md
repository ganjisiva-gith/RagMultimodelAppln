# RagMultimodelAppln

Simple RAG (retrieval-augmented generation) multi-model application implementation.

## Prerequisites

- Python 3.10+ installed
- Recommended: create and activate a virtual environment

## Setup

1. Create and activate a virtual environment (optional but recommended):

	````powershell
	python -m venv .venv
	.\.venv\Scripts\Activate.ps1
	````

2. Install dependencies:

	```powershell
	pip install -r requirements.txt
	```

3. (Optional) If the app uses external services (for example OpenAI), set required environment variables, e.g.:

	```powershell
	$env:OPENAI_API_KEY = "sk-..."
	```

## Running the application

This repository does not include a confirmed application entrypoint file by default. Follow one of the options below depending on your project state.

- If the project already contains a FastAPI app (a module that defines `app = FastAPI()`):

  Run it with Uvicorn (replace `module` with the module name that defines `app`):

  ```powershell
  uvicorn module:app --reload --host 0.0.0.0 --port 8000
  ```

- If there is no app file yet, create a minimal `app.py` in the repository root with the following content:

  ```python
  from fastapi import FastAPI

  app = FastAPI()

  @app.get("/")
  def read_root():
		return {"status": "ok"}
  ```

  Then run:

  ```powershell
  uvicorn app:app --reload --host 0.0.0.0 --port 8000
  ```

Notes:
- If your app file is named something else (for example `main.py`), replace `app` and `module` accordingly: `uvicorn main:app`.
- The `--reload` flag is intended for development only.

## Accessing the app

Once the server is running (default above uses port `8000`), open the following URLs in your browser or call them with `curl`:

- **Root:** http://localhost:8000/ — basic root endpoint (returns `{"status": "ok"}` for the minimal app)
- **OpenAPI docs (Swagger UI):** http://localhost:8000/docs — interactive API docs
- **ReDoc docs:** http://localhost:8000/redoc — alternative API docs UI
- **Health check:** http://localhost:8000/healthz — simple readiness/liveness check

Example `curl` commands:

```powershell
curl http://localhost:8000/
curl http://localhost:8000/healthz
```

If you started the server with `--host 0.0.0.0` and want to access it from another machine on the same network, replace `localhost` with the host machine's IP address (for example `http://192.168.1.100:8000`). Ensure any firewall allows incoming connections on that port.


## Running tests

Run the test suite with `pytest`:

```powershell
pytest -q
```

## Troubleshooting

- If `uvicorn` or `pytest` are not found, ensure the virtual environment is activated or install the packages into the active interpreter.
- If the app requires API keys or other secrets, set them as environment variables before starting the app.

## Next steps

- If you want, I can create a minimal `app.py` in the repo and start the server locally for you.

