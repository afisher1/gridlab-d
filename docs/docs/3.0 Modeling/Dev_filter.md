# Dev:filter - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Dev:filter
Dev:filter \- Filter implementation  PROPOSED FOR 4.0 

The [filter] directive is implemented using an augmented [transform] object. The filter is converted to a discrete-time state-space model using the canonical observable form for a strictly causal system: 

$$H(z) = \frac {\sum_{n=1}^{N-1} b_n z^{n-N}} {z^N + \sum_{n=1}^{N-1} a_n z^{n-N}} \to \left\\{ \begin{aligned} \dot x &= A x + B u \\\ y &= C x \end{aligned} \right. $$

where 

$$ A = \begin{bmatrix} 
    
    
      0 & 0 & \cdots & 0 & -a_n
    

\\\ 1 & 0 & \cdots & 0 & -a_{n-1} \\\ \vdots & \ddots & \ddots & \vdots & \vdots \\\ 0 & 0 & \ddots & 0 & -a_2 \\\ 0 & 0 & \cdots & 1 & -a_1 \end{bmatrix} \quad B = \begin{bmatrix} b_n \\\ b_{n-1} \\\ \vdots \\\ b_2 \\\ b_1 \end{bmatrix} \quad C = \begin{bmatrix} 0 & 0 & \cdots & 0 & 1 \end{bmatrix} $$

The state vector _x_ is update whenever the global clock modulo the time step is equal to the time skew. 

## See also

  * [filter]

