# PMOVES.AI Integration Guide for AgentGym

## Integration Overview

AgentGym provides 14+ diverse RL training environments (WebShop, WebArena, ALFWorld, SciWorld, BabyAI, etc.) for evaluating and evolving LLM-based agents. Within PMOVES.AI, it serves as the offline agent evaluation and self-evolution framework.

## Service Details

- **Name:** AgentGym
- **Slug:** agentgym
- **Tier:** worker
- **Port:** Per-environment (HTTP APIs per agentenv server)
- **Health Check:** Per-environment endpoint
- **NATS Enabled:** False
- **GPU Enabled:** True (for model training/inference)

## Integration Points

### Agent Evaluation Pipeline
- AgentGym environments are used by Agent Zero and Archon for offline agent quality assessment
- Trajectory datasets (AgentTraj-L) feed into Hi-RAG for knowledge retrieval training data
- AgentEvol self-evolution method can be triggered via NATS research events

### Data Flow
```
AgentGym Environments → Agent Trajectories → Hi-RAG Indexing
                      → AgentEval Benchmarks → Prometheus Metrics
```

## Next Steps

### 1. Customize Environment Variables

Edit the following files with your service-specific values:

- `env.shared` - Base environment configuration
- `env.tier-worker` - WORKER tier specific configuration

### 2. Integrate Health Check

Each agentenv server exposes HTTP endpoints:
- `POST /createEnv` - Create environment instance
- `POST /observation` - Get current observation
- `POST /step` - Take action in environment

### 3. Test Integration

```bash
# Verify environment server
curl http://localhost:<ENV_PORT>/createEnv

# Verify PMOVES environment variables loaded
docker compose exec agentgym env | grep PMOVES
```

## Files Created

- `PMOVES.AI_INTEGRATION.md` - This integration guide

## Support

For questions or issues, see the PMOVES.AI documentation.
