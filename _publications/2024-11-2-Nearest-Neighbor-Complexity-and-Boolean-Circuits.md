---
title: "Nearest Neighbor Complexity and Boolean Circuits"
collection: publications
permalink: /publication/2024-11-2-Nearest-Neighbor-Complexity-and-Boolean-Circuits
excerpt: 'This paper characterizes the number of data points required to represent Boolean functions via the nearest-neighbor rule.'
date: 2024-11-2
venue: 'ITCS 2025'
paperurl: 'http://masinister.github.io/files/nn.pdf'
---
**Abstract** A nearest neighbor representation of a Boolean function \( f \) is a set of vectors (anchors) labeled by \( 0 \) or \( 1 \) such that \( f(\vec{x}) = 1 \) if and only if the closest anchor to \( \vec{x} \) is labeled by \( 1 \). This model was introduced by Hajnal, Liu, and Turán (2022), who studied bounds on the minimum number of anchors required to represent Boolean functions under different choices of anchors (real vs. Boolean vectors) as well as the more expressive model of \( k \)-nearest neighbors.

We initiate the systematic study of the representational power of nearest and \( k \)-nearest neighbors through Boolean circuit complexity. To this end, we establish a close connection between Boolean functions with polynomial nearest neighbor complexity and those that can be efficiently represented by classes based on linear inequalities—*min-plus polynomial threshold functions*—previously studied in relation to threshold circuits. This extends an observation of Hajnal et al. (2022).

Next, we further extend the connection between nearest neighbor representations and circuits to the \( k \)-nearest neighbors case.

As an outcome of these connections, we obtain exponential lower bounds on the \( k \)-nearest neighbors complexity of explicit \( n \)-variate functions, assuming \( k \leq n^{1-\epsilon} \). Previously, no superlinear lower bound was known for any \( k > 1 \). At the same time, we show that proving superpolynomial lower bounds for the \( k \)-nearest neighbors complexity of an explicit function for arbitrary \( k \) would require a breakthrough in circuit complexity. In addition, we prove an exponential separation between the nearest neighbor and \( k \)-nearest neighbors complexity (for unrestricted \( k \)) of an explicit function. These results address questions raised by Hajnal et al. (2022) about proving strong lower bounds for \( k \)-nearest neighbors and understanding the role of the parameter \( k \). Finally, we devise new bounds on the nearest neighbor complexity for several families of Boolean functions.


[Download paper here](http://masinister.github.io/files/nn.pdf)
