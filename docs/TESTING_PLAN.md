# RAG System Container Testing Plan

## Overview
This document outlines the comprehensive testing plan for the containerized RAG system with CUDA 12.9.0 support.

## Build Information
- **Base Image**: `nvidia/cuda:12.9.0-cudnn-devel-ubuntu24.04`
- **Build Date**: July 1, 2025
- **CUDA Version**: 12.9.0
- **Target Architectures**: native (optimized for build machine)

## Pre-Testing Checklist
- [x] Updated Containerfile with CUDA 12.9.0
- [x] Fixed CUDA linker flags (-lcuda -lcudart)
- [x] Optimized build for native architecture
- [x] Created comprehensive test script (`test_container.sh`)
- [x] Created application launcher script (`launch_rag_app.sh`)

## Testing Phases

### Phase 1: Container Build Verification
**Objective**: Ensure the container builds successfully with all components

**Tests**:
1. ✅ **Container Image Creation**
   - Verify `rag-system:latest` image exists
   - Check image size and layers

2. ✅ **CUDA Environment**
   - Verify CUDA 12.9.0 toolkit installation
   - Test nvcc compiler availability
   - Check CUDA library paths

3. ✅ **llama.cpp Build**
   - Verify llama-server binary compilation
   - Test CUDA integration in build
   - Check for proper linking against CUDA libraries

4. ✅ **Python Environment**
   - Verify Python 3.12 installation
   - Test virtual environment creation
   - Validate kotaemon package installation

**Command**: `./test_container.sh`

### Phase 2: Container Runtime Testing
**Objective**: Verify container starts and runs correctly

**Tests**:
1. **Basic Container Startup**
   ```bash
   podman run -it --rm rag-system:latest /bin/bash
   ```

2. **Service Port Availability**
   ```bash
   podman run -d -p 8800:8800 -p 8801:8801 --name test-rag rag-system:latest sleep 60
   ```

3. **Volume Mounting**
   ```bash
   podman run -v ./test-data:/app/data:Z rag-system:latest ls -la /app/data
   ```

### Phase 3: Application Component Testing
**Objective**: Test individual components work correctly

**Tests**:
1. **llama.cpp Server**
   ```bash
   # Inside container:
   cd /app/llama.cpp/build
   ./bin/llama-server --help
   ```

2. **Python Dependencies**
   ```bash
   # Inside container:
   source /app/env/bin/activate
   python -c "import gradio, fastapi, langchain; print('Core deps OK')"
   ```

3. **Kotaemon Import**
   ```bash
   # Inside container:
   source /app/env/bin/activate
   cd /app/kotaemon
   python -c "import kotaemon; print('Kotaemon OK')"
   ```

### Phase 4: Web Application Testing
**Objective**: Launch and test the complete RAG web application

**Tests**:
1. **Application Startup**
   ```bash
   # Inside container:
   cd /app/kotaemon
   source /app/env/bin/activate
   python app.py
   ```

2. **Web Interface Access**
   - Test browser access to `http://localhost:8800`
   - Verify UI loads correctly
   - Test basic navigation

3. **RAG Functionality**
   - Test document upload
   - Test query processing
   - Verify response generation

### Phase 5: Performance and Integration Testing
**Objective**: Validate performance and full system integration

**Tests**:
1. **CUDA Acceleration (if GPU available)**
   - Test GPU utilization during inference
   - Compare performance vs CPU-only mode

2. **Memory Usage**
   - Monitor container memory consumption
   - Test with various document sizes

3. **Concurrent Users**
   - Test multiple simultaneous queries
   - Verify system stability under load

## Expected Results

### Successful Build Indicators
- ✅ No compilation errors
- ✅ All dependencies installed
- ✅ llama-server binary created
- ✅ kotaemon package importable

### Successful Runtime Indicators
- ✅ Container starts without errors
- ✅ Ports 8800/8801 accessible
- ✅ Web interface loads
- ✅ Can process sample queries

## Troubleshooting Guide

### Common Issues and Solutions

1. **CUDA Linker Errors**
   ```
   Error: undefined reference to cuMemRelease
   Solution: Ensure -lcuda -lcudart flags in CMAKE_*_LINKER_FLAGS
   ```

2. **Python Import Errors**
   ```
   Error: ModuleNotFoundError
   Solution: Activate virtual environment before running Python code
   ```

3. **Port Binding Issues**
   ```
   Error: Port already in use
   Solution: Check for existing containers: podman ps -a
   ```

4. **Permission Errors with Volumes**
   ```
   Error: Permission denied
   Solution: Use :Z flag for SELinux contexts: -v ./data:/app/data:Z
   ```

## Automated Testing Scripts

### Quick Test
```bash
./test_container.sh
```

### Interactive Launch
```bash
./launch_rag_app.sh
```

### Manual Commands
```bash
# Build
podman build -t rag-system:latest -f Containerfile .

# Test run
podman run -it --rm -p 8800:8800 -p 8801:8801 rag-system:latest

# Inside container
cd /app/kotaemon && source /app/env/bin/activate && python app.py
```

## Success Criteria

The testing is considered successful when:
- [x] Container builds without errors
- [ ] All automated tests pass
- [ ] Web application starts successfully
- [ ] Can process at least one RAG query
- [ ] No critical errors in logs
- [ ] Performance is acceptable for intended use

## Next Steps After Testing

1. **Documentation Update**: Update README with final testing results
2. **Performance Optimization**: Based on test results, optimize configuration
3. **Production Deployment**: Prepare for production environment if needed
4. **User Documentation**: Create user guide for running the system

---

**Testing Status**: 🟡 In Progress
**Last Updated**: July 1, 2025
**Next Review**: After container build completion 