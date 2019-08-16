---
layout:     post
title:      "Image Style Transfer Using CNN"
subtitle:   " "
date:       2019-06-26 16:00:00 +0900
background: '/img/universe.jpg'
comments:   true
categories: paper
---


&emsp;미술 작품과 실제 사진을 합성하여 사진을 그 작품의 화풍으로 표현하는 기술을 다룬 논문이다. 이를 image style transfer라고 하는데, 간단히 말하자면 그림의 style과 사진의 content를 이용하여 새로운 그림을 생성하는 것이다.



<br>

#### 2. Deep image representations
&emsp;이 논문에서 사용된 모델은 VGG network를 기반으로 만들어졌다. VGG는 convolutional layer와 pooling layer로 구성되는 기본적인 CNN으로, 층의 깊이에 따라서 VGG16, VGG19로 구분하기도 한다. 여기서는 19-layer VGG network를 사용하였고, 16개의 convolutional layer와 5개의 pooling layer로 구성되었다. 일반적으로 사용하는 max pooling보다 average pooling의 성능이 좋아 average pooling을 사용하였다.
 <!--이 논문의 결과는 object recognition과 localisation을 수행하기 위해 학습된 VGG network를 기반으로 만들어졌다. Fully connected layer는 사용하지 않았다.-->

<br>



##### 2.1 Content representation
<!--일반적으로 네트워크의 각 레이어는 네트워크에서의 레이어의 위치에 따라 복잡도가 증가하는 non-linear filter bank를 정의한다. 따라서 주어진 input image $$\vec{x}$$는 filter를 적용했을 때의 결과로 CNN의 각 레이어에 인코드된다.-->
&emsp;일반적으로 네트워크에서는 레이어의 위치에 따라서 추출되는 feature가 달라진다.
<img src="/img/paper/image-style-transfer-cnn.png" alt="cnn" width="100%">
<br>
$$N_l$$개의 서로 다른 필터를 가지는 레이어는 사이즈가 $$M_l$$인 $$N_l$$개의 feature map을 가진다. 따라서 layer $$l$$의 결과는 행렬 $$F_l \in \mathcal R^{N_l\times M_l}$$에 저장된다.
* $$M_l$$은 feature map의 높이와 너비의 곱
* $$F_{ij}^l$$은 layer $$l$$의 $$j$$번째 위치에서의 $$i$$번째 필터의 activation

<!--서로 다른 각각의 레이어에 인코드된 이미지 정보를 시각화하기 위해서는 white noise image에 gradient descent를 수행하여 원래 이미지의 feature response와 매치되는 또다른 이미지를 찾을 수 있다.-->
&emsp;각 레이어마다 인코딩된 이미지 정보를 시각화하기 위해서 white noise image를 이용하였다. $$\vec{p}$$를 original image, $$\vec{x}$$를 생성된 이미지라고 하자. $$P_l$$과 $$F_l$$은 각각 layer $$l$$에서의 feature representation이다. 두 feature representation에 대해서 squared-error loss를 다음과 같이 정의하고, 이 값이 작아지도록 $$\vec{x}$$를 조절하여 $$\vec{p}$$와 비슷한 이미지를 만드는 것이다.

$$L_{content}(\vec{p},\vec{x},l) = \frac12\sum_{i,j}(F_{ij}^l-P_{ij}^l)^2$$

이 loss값을 layer $$l$$의 activation 값에 대해 미분한 값은

$$ \frac{\partial L_{content}}{\partial F_{ij}^l} =
\begin{cases}
(F^l - P^l)_{ij} & \text{if $F_{ij}^l > 0$} \\
0 & \text{if $F_{ij}^l < 0$}
\end{cases}
$$

<!--image $$\vec{x}$$에 대한 gradient는 standard error back-propagation을 이용하여 계산될 수 있다. 따라서 우리는 random image $$\vec{x}$$가 CNN의 특정 layer에서 original image $$\vec{p}$$와 같은 결과를 생성할 때까지 이 $$\vec{x}$$를 바꿀 수 있다.-->
&emsp;깊은 layer로 들어갈수록 input image의 정확한 픽셀값보다는 content 정보를 더 많이 가지게 된다. 여기서는 content representation이 목적이므로 higher layer의 feature response를 사용한다.<!--CNN이 object recognition에서 학습될 때, processing hierarchy 에 따라서 object information을 만드는데, 이때 input image는 이미지의 실제 content에 sensitive하고 그것의 appearance에 대해서는 invariant한 representation으로 변한다. 따라서 네트워크의 higher layer에서는 object와 input image의 arrangement에 대해서 high level content를 포착하고 정확한 픽셀값에 대해서는 크게 집착하지 않게 된다. 반면에 lower layer에서 reconstruction이 이루어질 때는 원래 이미지의 정확한 픽셀값을 단순히 재생산하는 것이 된다.-->

<br>

##### 2.2 Style representation
&emsp;Texture(style) 정보를 얻기 위해서 Gram matrix를 사용하였다. 이것은 각기 다른 filter response 간의 correlation으로 구성된 행렬이다.
<!--input image에서 style이라는 representation을 얻기 위해서는 texture information을 얻기 위해 고안된 feature space를 사용한다. 이것은 각기 다른 filter response 간의 correlation으로 구성되는데, where the expectation is taken over the spatial extent of the feature maps. 이 feature의 correlation을 얻기 위해 Gram matrix $$G^l \in \mathcal R^{N_l \times N_l}$$라는 것을 사용하였다.-->


$$G^l_{ij} = \sum_{k}F^l_{ik}F^l_{jk}\;, \qquad G^l \in \mathcal R^{N_l \times N_l}$$

여기서 $$G^l_{ij}$$는 layer $$l$$의 vectorised feature map인 $$i$$와 $$j$$의 내적이다.
<!--여러 layer의 feature correlation을 이용하여, input image의 global arrangement는 아니지만 texture 정보를 얻을 수 있는 stationary, multi-scale representation을 얻을 수 있다. 다시 말하면, 주어진 input image의 style representation과 매칭되는 image를 구성함으로써(construct), 네트워크의 서로 다른 레이어에서 만들어진 style feature space에서 포착할 수 있는 정보를 시각화할 수 있다.
(Again, we can visualise the information captured by these style feature spaces built on different layers of the network by constructing an image that matches the style representation of a given input image.) ~~그러니까 인풋 이미지와 비슷한? 어떤 이미지를 만들어내고 각 레이어에서 계속해서 feature를 찾아내는데? 각 레이어마다 담고 있는 정보를 시각화해서 보여줄 수 있다고 어떤 이미지(하나?)를 만들어서..~~ -->White noise image에 대해 gradient를 계산하고 original image의 Gram matrix와 생성된 image의 Gram matrix의 entry 간의 거리의 평균 제곱값을 최소화함으로써 style 정보를 나타낼 수 있다.
$$\vec{a}$$를 original image, $$\vec{x}$$를 생성될 이미지라고 하자. $$A^l$$과 $$G^l$$는 각각 layer $$l$$에서의 style representation이다. Total loss에 대해 layer $$l$$의 기여도는

$$E_l = \frac{1}{4N^3_lM^2_l}\sum_{i,j}(G^l_{ij} - A^l_{ij})^2$$

이고 total style loss는

$$ \mathcal L_{style}(\vec{a}, \vec{x}) = \sum_{l=0}^L w_lE_l $$

이다. $$w_l$$은 total loss에 대해 각 layer가 얼마나 기여하는지를 나타내는 weighting factor이다.

$$E_l$$를 layer $$l$$의 활성값에 대해 미분한 값은

$$ \frac{\partial E_l}{\partial F^l_{ij}} =
\begin{cases}
    \frac{1}{N_l^2M_l^2}((F^l)^T(G^l-A^l))_{ji} & \text{if $F^l_{ij}>0$} \\
    0 & \text{if $F^l_{ij} < 0$}
\end{cases}
$$

<br>

##### 2.3 Style transfer

&emsp;미술작품 $$\vec{a}$$의 style을 사진 $$\vec{p}$$에 옮기기 위해서는 $$\vec{p}$$의 content representation, $$\vec{a}$$의 style representation과 동시에 매칭되는 새로운 이미지를 합성해야 한다. 따라서 white noise $$\vec{x}$$를 사진의 content representation과 비슷하게 만들면서 white noise와 미술작품의 style representation을 비슷하게 만들어야 한다. <!--하나의 레이어에서 사진의 content representation에서 나온 white noise의 feature representation과 2) CNN의 여러 layer에서 정의된 미술작품의 style representation의 거리를 최소화할 것이다.-->최소화해야 하는 loss function은

$$
\mathcal L_{total}(\vec{p},\vec{a},\vec{x})
 = \alpha \mathcal L_{content}(\vec{p},\vec{x}) + \beta \mathcal L_{style}(\vec{a}, \vec{x})
$$

$$\alpha$$와 $$\beta$$는 각각 content와 style reconstruction에 대한 가중치이다. <!--픽셀값에 대한 gradient $$ \frac{\partial \mathcal L_{total}}{\partial \vec{x}}$$은 어떤 numerical optimisation strategy로 사용될 수 있다. 여기서는 L-BFGS를 사용하는데, 이것은 이미지 합성에서 가장 성능이 좋다.--> 비교할 수 있는 scale로 이미지 정보를 추출하기 위해서 feature representation을 계산하기 전에 항상 style image와 content image를 같은 크기로 resize해야 한다. <!--마지막으로 여기서는 image prior로 합성을 정규화하지 않았다. lower layer의 texture feature가 style image의 특정한 image prior로 활용될 수 있는지는 논쟁이 있을 수 있는 부분이다. 덧붙여 어떤 네트워크 구조와 최적화 알고리즘을 사용하는지에 따라 이미지 합성의 결과가 달라질 수도 있다.-->


<br>
<img src="/img/paper/image-style-transfer-algorithm.png" alt="algorithm" width="100%">

<br><br>

#### 3. Results

<img src="/img/paper/image-style-transfer-result.png" alt="result" width="100%">
(A) the Neckarfront in Tübingen, Germany<br>
(B) *The Shipwreck of the Minotaur* by J.M.W. Turner, 1805<br>
(C) *The Starry Night* by Vincent van Gogh, 1889<br>
(D) *Der Schrei* by Edvard Munch, 1893<br>
(E) *Femme nue assise* by Pablo Picasso, 1910<br>
(F) *Composition VII* by Wassily Kandinsky, 1913<br>

<!--
이 논문의 중요한 발견은 CNN에서 content와 style의 representation은 분리가 가능하다는 점이다. 즉, 두 representation을 독립적으로 조작해서 새로운, 의미 있는 이미지를 만들 수 있다는 것이다. 이러한 발견을 증명하기 위해서 서로 다른 두 개의 source image에서 content와 style representation을 합성하여 새로운 이미지를 생성하였다. 독일 튀빙겐의 Nektar 강변의 이미지의 content representation과 여러 시대의 유명한 미술 작품들의 style representation을 이용하였다.

<br>

##### 3.1 Trade-off between content and style matching

이미지를 합성할 때, content와 style에 완벽하게 들어맞는 이미지를 만들 수는 없다. 그러나 이미지를 합성할 때 사용하는 loss function이 content와 style의 linear combination이기 때문에 이를 정규화하여 둘 중 하나를 더 강조할 수는 있다. Style을 더 강조하면 생성된 이미지는 예술작품과 더 가까운 그림이 되고, 사진의 content는 덜 나타나게 된다. Content를 강조하면 사진의 content는 확실하게 알아볼 수 있지만 작품의 style은 찾아보기가 힘들다. 시각적으로 좋은 그림을 생성하려면 이러한 trade-off를 고려해야 한다.

<br>

#### 3.2 Effect of different layers of the Convolutional Neural Network

이미지 합성 과정에서 또다른 중요한 요인은 content와 style representation을 매칭시킬 layer를 정하는 것이다. 위에서 대략 언급했지만 style representation은 신경망의 여러 레이어를 포함하는 multi-scale representation이다. 이 layer들의 수와 위치는 어떤 style이 매칭되는 local scale을 결정하고, 따라서 결과물도 달라지게 된다. style representation을 신경망의 higher layer에 매칭하는 것은 local image 구조를 large scale로 유지해서 부드럽고 연속적인 시각적인 느낌을 만들어낸다는 것을 알아냈다. 따라서 시각적으로 좋은 그림은 보통 style representation을 네트워크에서 high layer와 매칭함으로써(??) 생성할 수 있다. 이러한 이유로 모든 이미지를 layer 'conv1_1', 'conv2_1', 'conv3_1', 'conv4_1', 'conv5_1'에 style feature를 매칭한 것이다.
Content feature를 서로 다른 layer를 사용하여 매칭한 효과를 분석하기 위해, 같은 미술작품과 같은 parameter값($$\alpha / \beta = 1 \times 10^{-3}$$)을 설정하고, 'conv2_2'와 'conv4_2'의 결과를 보여주었다. 네트워크의 lower layer에 content를 매칭할 때는 알고리즘이 사진에서 디테일한 픽셀 정보와 매칭되었고, 결과물은 사진에 미술작품의 질감이 단순히 섞인 것처럼 나왔다. 반면에, 네트워크의 higher layer에 content feature를 매칭했을 때는 사진의 디테일한 픽셀 정보는 크게 강조되지 않고 사진의 content가 적절하게 합성되었다. 즉, 이미지의 세밀한 구조는 바뀔 수 있다.

<br>

#### 3.3 Initialisation of gradient descent

지금까지 보여준 모든 이미지는 white noise로 initialise한 것이었다. 그러나 content image나 style image로 initialise할 수도 있다. 결과가 초기값에 따라 조금씩 달라질 수는 있지만 initialisation 방법이 합성 결과에 큰 영향을 미치지는 않는 것을 확인할 수 있다. noise로 initialise하는 것만이 임의의 개수의 새로운 이미지를 생성하는 것을 가능하게 한다.

<br>

#### 3.4 Photorealistic style transfer

지금까지는 미술작품의 style transfer만을 다루었지만 임의의 사진을 이용해서도 style transfer를 할 수 있다. 뉴욕의 밤 사진 style을 런던의 낮 사진으로 transfer하면 런던 사진을 밤에 찍은 듯한 사진을 얻을 수 있다.
-->


<br>
<br>

여러 사진을 이용해 직접 실험을 해보았다.

<img src="/img/paper/image-style-transfer-vangogh_starry_night.jpg" alt="renoir" width="30%"> $$+$$
<img src="/img/paper/image-style-transfer-disney-square.png" alt="ikseon" width="30.9%"> $$=$$
<img src="/img/paper/image-style-transfer-gogh-disney-square.png" alt="renoir+ikseon" width="30%">

<br>

<img src="/img/paper/image-style-transfer-renoir.jpg" alt="renoir" width="30%"> $$+$$
<img src="/img/paper/image-style-transfer-ikseon.png" alt="ikseon" width="30.9%"> $$=$$
<img src="/img/paper/image-style-transfer-renoir-ikseon.png" alt="renoir+ikseon" width="30%">

작품을 바꿔가면서 이미지 합성을 해보았는데 색감이 비슷한 작품과 사진을 합성했을 때 결과가 더 좋은 것 같았다.

<br><br><br>

[참고] <a href="https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/Gatys_Image_Style_Transfer_CVPR_2016_paper.pdf" target="_blank">Image Style Transfer Using Convolutional Neural Networks</a>
