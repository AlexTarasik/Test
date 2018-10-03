# Test
<?php

namespace Drupal\sat_test\Form;

use Drupal\Core\Form\FormBase;
use Drupal\Core\Form\FormStateInterface;

class EmailForm extends FormBase{ 
    public function buildForm(array $form, FormStateInterface $form_state){ // метод, который отвечает за саму форму - кнопки, поля
        $form['title'] = [
            '#type' => 'textfield',
            '#title' => $this->t('First name'),
            '#description' => $this->t('Имя не должно содержать цифр'),
            '#required' => TRUE,
        ]; 
        $form['titlelast'] = [
            '#type' => 'textfield',
            '#title' => $this->t('Last name'),
            '#description' => $this->t(''),
            '#required' => TRUE,
        ]; 
        $form['titleSubject'] = [
            '#type' => 'textfield',
            '#title' => $this->t('Subject'),
            '#description' => $this->t(''),
            '#required' => TRUE,
        ]; 
        $form['titleMesssage'] = [
            '#type' => 'textarea',
            '#title' => $this->t('Message'),
            '#description' => $this->t('formatted, long'),
            '#required' => TRUE,
        ]; 
        $form['actions']['submit'] = [
            '#type' => 'submit',
            '#value' => $this->t('Отправить форму'),
        ];
        return $form;
    }
    
    public function submitForm(array &$form, FormStateInterface $form_state) { // действия по сабмиту
        $mailManager = \Drupal::service('plugin.manager.mail');
        $module = 'sat_test';
        $key = 'sat_test_mail';
        $to = $form_state->getValue('email');
        $params['message'] = $form_state->getValue('message');
        $params['subject'] = $form_state->getValue('subject');
        $langcode = \Drupal::currentUser()->getPreferredLangcode();
        $send = true;
        $result = $mailManager->mail($module, $key, $to, $langcode, $params, NULL, $send);
        if ($result['result'] !== true) {
            drupal_set_message(t('There was a problem sending your message and it was not sent.'), 'error');
          }
          else {
            drupal_set_message(t('Your message has been sent.'));
          }
    }
    public function getFormId() {
        return 'sat_test_exform_form'; // метод, который будет возвращать название формы
    }
    public function validateForm(array &$form, FormStateInterface $form_state) { // ф-я валидации
        $title = $form_state->getValue('title');
        $is_number = preg_match("/[\d]+/", $title, $match);

        if ($is_number > 0) {
         $form_state->setErrorByName('title', $this->t('Строка содержит цифру.'));
        }
    }
} 
    foreach ($email as $item) {
        if (1 == preg_match(
            '/^((([0-9A-Za-z]{1}[-0-9A-z\.]{1,}[0-9A-Za-z]{1})|([0-9А-Яа-я]{1}[-0-9А-я\.]{1,}[0-9А-Яа-я]{1}))@([-0-9A-Za-z]{1,}\.){1,2}[-A-Za-z]{2,})$/u', $item) . '<br/>'){
            echo '"' . $item . '" : correct' . '<br/>';
        } else {
            echo '"' . $item . '" : non-correct' . '<br/>';
        }
    }
