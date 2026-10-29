# Tasks: `04-a2a-microservices`

## 1. A2A protocol package

- [ ] 1.1 Copy `backup/cs_agent/a2a/{__init__.py,types.py,utils.py,task_manager.py,server.py,factories.py,client.py,run_servers.py}` to `src/a2a/`, bind Judge/Masker from `A2A_JUDGE_HOST`/`A2A_JUDGE_PORT` and `A2A_MASK_HOST`/`A2A_MASK_PORT`, and verify with `pytest tests/integration/test_a2a.py`

## 2. Security agents

- [ ] 2.1 Copy `backup/cs_agent/agents/{__init__.py,judge_agent.py,mask_agent.py,pipeline.py}` to `src/agents/`, call `mask_sensitive_data(text)` with no `GOOGLE_CLOUD_PROJECT`, and verify discovery cards plus `tasks/send`, `tasks/sendSubscribe`, `tasks/get`, `tasks/cancel`, `-32601`, and `-32000` with `pytest tests/integration/test_a2a.py`
