# snow

Victor Cristiano do Nascimento Silva  21080416
Daniel Espindola da Silva  11201720556


A ideia era fazer um vitrine de estatuas a camera mexesse apenas na horizontal.

Colocamos os bonecos de neve enfileirados representando as estatuas.

posicionamos eles configurando a matriz do obj
ex: model = glm::translate(model, glm::vec3(-4.0f, 0.0f, 0.0f));

Inicialmente iriamos colocar a camera em uma posicao de uma pessoa vendo as estatuas, mas vimos que perdiamos o chao, entao decidimos mudar a camera para visualização de cima, conforme mostra o codigo.

private:
  glm::vec3 m_eye{0.0f, 2.0f, 2.5f}; // Camera position
  glm::vec3 m_at{0.0f, 0.5f, 0.0f};  // Look-at point
  glm::vec3 m_up{0.0f, 1.0f, 0.0f};  // "up" direction

Para ter o movimento horizontal, trocamos por onde era dada a entrada do comando teclas E e Q, para as setas direita e esquerda. Para isso ser feito mativemos apenas a variavel truck e tiramos o movimento dolly e pan, pois rotacionavam e moviam a camera pra frente.

void Camera::truck(float speed) {
  // Compute forward vector (view direction)
  auto const forward{glm::normalize(m_at - m_eye)};
  // Compute vector to the left
  auto const left{glm::cross(m_up, forward)};

  // Move eye and center to the left (speed < 0) or to the right (speed > 0)
  m_at -= left * speed;
  m_eye -= left * speed;

  computeViewMatrix();
}

Para fazer o movimento dos objetos nós criamos duas variáveis no window, para controlar a rotação e o movimento vertical.
No onUpdate nós atualizamos essas variáveis aumentando ou diminuindo para gerar o movimento.
No onPaint nós determinamos a altura dos objs, alterando entre somar ou subtrair a deslocação de altura e giramos os objs
