---
layout: page
title: VoluStream - Reliable Volumetric Video Streaming System for Videoconferencing
description: Ongoing work
img: assets/img/volustream_thumb.png
importance: 1
category: work
related_publications: false
---

<!--
We present **VoluStream**, a novel, hybrid loss-recovery framework for reliable volumetric video streaming for videoconferencing. To the best of our knowledge, this is the first end-to-end system for this task. Its core design combines: (1) A novel, codec-agnostic, client-end neural framework for recovering corrupted RGB and Depth frames (2) A reliable transport-layer mechanism for critical frames. -->

### Abstract

The adoption of volumetric video for immersive applications is growing, driven by its support for six-degree-of-freedom (6-DoF) interaction. A major obstacle to its widespread use—particularly for real-time telepresence—is the challenge of streaming large data payloads over lossy networks without degrading the Quality of Experience (QoE).

To address this, we propose **VoluStream**, a novel framework for loss-resilient volumetric video streaming. Our system integrates a transport-layer strategy with an application-layer recovery engine. We first segregate frames into critical and non-critical sets; critical frames are sent via a reliable protocol, while non-critical ones are sent unreliably to minimize latency. Any resulting packet loss in the non-critical stream is handled by our Vision Transformer (ViT)-based recovery module, trained to restore both RGB and depth information.

### The Challenge

Volumetric content (Point clouds, RGB-D, NeRFs) demands high storage and bandwidth. Existing works typically focus on:

1.  **Optimization:** Improving encoder-decoder frameworks, which often neglects storage costs.
2.  **Compression:** Designing neural compression frameworks that assume perfect network conditions.

However, networks are inherently lossy. Standard recovery methods like Retransmission (too slow for real-time) or Forward Error Correction (bandwidth heavy) are ill-suited for volumetric data. Furthermore, existing neural recovery methods (e.g., GRACE, REPARO) focus exclusively on 2D RGB video, failing to address the interdependency of Depth frames required for 3D reconstruction.

### The VoluStream Approach

We propose a unified, hybrid framework that leverages the strengths of both transport and application layers.

1.  **Hybrid Transport:** We characterize frames as "critical" or "non-critical." Critical frames are sent over a reliable transport stream (TCP), while non-critical frames utilize an unreliable stream (QUIC over UDP) to minimize latency.
2.  **Neural Recovery:** A novel ViT-based loss recovery module operates at the client side. It is specifically trained to reconstruct lost non-critical RGB and Depth frames, ensuring high fidelity even under packet loss.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <video 
            width="100%" 
            controls 
            autoplay 
            loop 
            muted 
            class="img-fluid rounded z-depth-1">
            <source src="{{ 'assets/video/out_vis_rgb.mp4' | relative_url }}" type="video/mp4">
            Your browser does not support the video tag.
        </video>
    </div>
    
    <div class="col-sm mt-3 mt-md-0">
        <video 
            width="100%" 
            controls 
            autoplay 
            loop 
            muted 
            class="img-fluid rounded z-depth-1">
            <source src="{{ 'assets/video/out_vis_depth.mp4' | relative_url }}" type="video/mp4">
            Your browser does not support the video tag.
        </video>
    </div>
</div>
<div class="caption">
    <strong>Comparison of (a) RGB Reconstruction and (b) Depth Reconstruction under lossy network conditions.</strong>
</div>

### System Architecture

Our prototype is built on a WebRTC system that achieves real-time performance (over 30 FPS) for videoconferencing applications. The architecture ensures that critical dependencies for 3D reconstruction are preserved while non-essential data is recovered neurally.

### Key Contributions

- **First End-to-End System:** To the best of our knowledge, VoluStream is the first system designed specifically for reliable volumetric video streaming.
- **Codec-Agnostic Recovery:** A novel client-side neural framework that recovers corrupted RGB and Depth frames, compatible with existing compression codecs.
- **Real-Time Performance:** A prototype WebRTC implementation demonstrating >30 FPS performance suitable for interactive videoconferencing.
