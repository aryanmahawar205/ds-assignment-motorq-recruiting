
# Vehicle Event & Charge Analytics - Integrated EDA Solution

## Enhanced Approach: Integrated Event Analysis

This solution implements an integrated approach where **both ignition events and charging status events serve as inputs to the charging event detection**, following best practices for multi-source telematics analysis.

## Discovered Schemas & Data Issues

### Dataset Structures
- **TRG**: 34,811 events | Columns: ['Unnamed: 0', 'CTS', 'PNID', 'NAME', 'VAL']
- **TLM**: 18,885 records | Ignition data: 12.1% available
- **SYN**: 411 ground truth events
- **MAP**: 19 vehicle mappings | Coverage: 68.4%

### Critical Issues Identified
1. **High TLM sparsity**: 87.5% missing ignition data
2. **Vehicle mapping gaps**: TRG charging sensors not linked to vehicles
3. **Timestamp variations**: Multiple timezone formats requiring standardization
4. **Data integration**: Limited cross-source vehicle overlap for validation

## Enhanced Design Choices

### Integrated Multi-Source Event Processing
- **TRG**: Primary source for volume (15,737 ignition events)
- **TLM**: Vehicle-level validation (83 state changes)
- **SYN**: Ground truth validation (411 expert-labeled events)
- **Integration**: Both ignition and charging status events used as inputs to final detection

### Context-Aware Charging Detection
- **Ignition Context Integration**: Uses ignition events to determine engine state during charging
- **Dynamic Thresholds**: Stricter criteria when ignition is ON (>10% battery, >10min) vs OFF (>5% battery, >5min)
- **Multi-Source Correlation**: Correlates charging sessions with ignition state within ±30 minute windows

### Battery Association Logic
- **±300 second window**: Per specification requirement
- **PNID-based matching**: Sensor-level correlation when vehicle mapping unavailable
- **Tie-breaker**: First occurrence using pandas idxmin() for consistency

## Results Summary
- **Ignition Events**: 16,231 events extracted from 3 sources
- **Charging Events**: 89 successful sessions identified with ignition context
- **Integration Success**: Now using both event types as inputs to charging detection
- **Context-Aware Logic**: Applied stricter thresholds for ignition-ON charging sessions
- **Battery Associations**: 14,750 ignition + 2,794 charging events correlated

## What I'd Improve Next (≤300 words)

**Enhanced Vehicle Attribution**: Implement advanced entity resolution to better link TRG sensor data with vehicle identities, potentially using temporal correlation patterns and geographic clustering to infer vehicle-sensor relationships.

**Real-Time Integration**: Develop streaming architecture that processes ignition and charging events together in real-time, enabling immediate detection of anomalous charging patterns (like charging while driving) for safety alerts.

**Advanced Ignition Context**: Expand ignition state determination beyond simple ON/OFF to include transitional states, engine warm-up periods, and hybrid vehicle modes, providing more nuanced context for charging behavior analysis.

**Predictive Analytics**: Use the integrated ignition-charging patterns to build predictive models for charging demand, optimal charging times, and infrastructure utilization, leveraging the temporal relationships between ignition events and charging needs.

**Cross-Source Validation Enhancement**: Implement statistical correlation analysis between TRG, TLM, and SYN sources to quantify detection accuracy and develop confidence scores for each charging event based on supporting evidence from multiple sources.

**Dynamic Threshold Learning**: Replace static thresholds with machine learning models that adapt based on historical charging patterns, vehicle types, and environmental factors, improving detection accuracy across diverse scenarios.

The integrated approach successfully demonstrates how multi-source event correlation can enhance charging event detection accuracy while providing richer business context through ignition state awareness.
