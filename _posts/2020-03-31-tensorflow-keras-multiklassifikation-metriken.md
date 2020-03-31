---
title: Imbalanced Multiclass Classification with Tensorflow Keras
toc: true
note: true
updated: 2020-03-31 21:07
teaser: Mit Tensorflow und Keras können Klassifizierungs-Probleme mit wenig Aufwand, und noch weniger Code gelöst werden. Die entsprechenden MNIST oder Iris Beispiele enthalten dabei jedoch meist einen ausgeglichenen Datensatz. Ist dies nicht der Fall, so gibt es einige Herausforderungen.
---

Will man ein Klassifikationsproblem mit Tensorflow und Keras lösen, bei welchem die Trainingsdaten Labels nicht gleichmässig verteilt sind, so treten schnell Probleme auf. Die Metriken, welche in den meisten Tutorials aufgeführt sind, bieten sich nur für ausgeglichene (*balanced*) Datensätze an, nicht aber für unausgeglichene (*imbalanced*). Dieses Problem wird meist mit *down-* oder *upsampling* Techniken bzw. der vergabe von *class_weight*'s  gelöst, obwohl dies nicht wirklich der Realität entspricht. Tritt ein Label innerhalb der Testdaten weniger hufig auf, so sollte das Neuronale Netz dies auch entsprechend erlernen und bei Vorhersagen berücksichtigen.

> Imbalanced data classification is an inherantly difficult task since there are so few samples to learn from. You should always start with the data first and do your best to collect as many samples as possible and give substantial thought to what features may be relevant so the model can get the most out of your minority class. At some point your model may struggle to improve and yield the results you want, so it is important to keep in mind the context of your problem and the trade offs between different types of errors.

<a href="https://www.youtube.com/watch?v=HBi-P5j0Kec" target="_blank">Dieses Video</a> erklärt die sehr gut.

## Metriken während dem Training: Callbacks

Mir haben folgende *Keras Callbacks* sehr geholfen um während dem Training eines Modelles nicht allein die *Accuracy* im Auge zu behalten sondern die "wirkliche performance". Dabei werden die wichtigsten Werte viduell dargestellt und geplottet (ohne Verwendung von TensorBoard).

```python
import matplotlib.pyplot as plt    
import matplotlib.patches as mpatches  
from sklearn.metrics import confusion_matrix
from sklearn.metrics import classification_report
import itertools
import numpy as np

class AccLossPlotter(tf.keras.callbacks.Callback):
    """Plot training Accuracy and Loss values on a Matplotlib graph. 
    The graph is updated by the 'on_epoch_end' event of the Keras Callback class

    Adapted from: https://github.com/chasingbob/keras-visuals/blob/master/visual_callbacks.py

    # Arguments
        graphs: list with some or all of ('acc', 'loss')
        save_graph: Save graph as an image on Keras Callback 'on_train_end' event 
    """
    def __init__(self, graphs=['acc', 'loss'], save_graph=False):
        self.graphs = graphs
        self.num_subplots = len(graphs)
        self.save_graph = save_graph

    def on_train_begin(self, logs={}):
        self.acc = []
        self.val_acc = []
        self.loss = []
        self.val_loss = []
        self.epoch_count = 0
        plt.ion()
        plt.show()

    def on_epoch_end(self, epoch, logs={}):
        self.epoch_count += 1
        self.val_acc.append(logs.get('val_categorical_accuracy'))
        self.acc.append(logs.get('categorical_accuracy'))
        self.loss.append(logs.get('loss'))
        self.val_loss.append(logs.get('val_loss'))
        epochs = [x for x in range(self.epoch_count)]

        count_subplots = 0
        
        if 'acc' in self.graphs:
            count_subplots += 1
            plt.subplot(self.num_subplots, 1, count_subplots)
            plt.title('Accuracy')
            plt.plot(epochs, self.val_acc, color='r')
            plt.plot(epochs, self.acc, color='b')
            plt.ylabel('accuracy')

            red_patch = mpatches.Patch(color='red', label='Val')
            blue_patch = mpatches.Patch(color='blue', label='Train')

            plt.legend(handles=[red_patch, blue_patch], loc=4)

        if 'loss' in self.graphs:
            count_subplots += 1
            plt.subplot(self.num_subplots, 1, count_subplots)
            plt.title('Loss')
            plt.plot(epochs, self.val_loss, color='r')
            plt.plot(epochs, self.loss, color='b')
            plt.ylabel('loss')

            red_patch = mpatches.Patch(color='red', label='Val')
            blue_patch = mpatches.Patch(color='blue', label='Train')

            plt.legend(handles=[red_patch, blue_patch], loc=4)
        
        plt.draw()
        plt.pause(0.001)

    def on_train_end(self, logs={}):
        if self.save_graph:
            plt.savefig('training_acc_loss.png')

class ConfusionMatrixPlotter(tf.keras.callbacks.Callback):
    """Plot the confusion matrix on a graph and update after each epoch

    Adapted from: https://github.com/chasingbob/keras-visuals/blob/master/visual_callbacks.py

    # Arguments
        X_val: The input values 
        Y_val: The expected output values
        classes: The categories as a list of string names
        normalize: True - normalize to [0,1], False - keep as is
        cmap: Specify matplotlib colour map
        title: Graph Title
    """
    def __init__(self, X_val, Y_val, classes, normalize=False, cmap=plt.cm.Blues, title='Confusion Matrix'):
        self.X_val = X_val
        self.Y_val = Y_val
        self.title = title
        self.classes = classes
        self.normalize = normalize
        self.cmap = cmap
        plt.ion()
        plt.figure()
        plt.title(self.title)

    def on_train_begin(self, logs={}):
        pass
    
    def on_epoch_end(self, epoch, logs={}):    
        plt.clf()
        pred = self.model.predict(self.X_val)
        max_pred = np.argmax(pred, axis=1)
        max_y = np.argmax(self.Y_val, axis=1)
        cnf_mat = confusion_matrix(max_y, max_pred)
   
        if self.normalize:
            cnf_mat = cnf_mat.astype('float') / cnf_mat.sum(axis=1)[:, np.newaxis]

        thresh = cnf_mat.max() / 2.
        for i, j in itertools.product(range(cnf_mat.shape[0]), range(cnf_mat.shape[1])):
            plt.text(j, i, cnf_mat[i, j],                                          
                         horizontalalignment="center",
                         color="white" if cnf_mat[i, j] > thresh else "black")

        plt.imshow(cnf_mat, interpolation='nearest', cmap=self.cmap)

        # Labels
        tick_marks = np.arange(len(self.classes))
        plt.xticks(tick_marks, self.classes, rotation=45)
        plt.yticks(tick_marks, self.classes)
        plt.colorbar()                                                                       
        plt.tight_layout()                                                    
        plt.ylabel('True label')                                              
        plt.xlabel('Predicted label')                                         
        plt.show()
        plt.pause(0.001)


class ClassificationReport(tf.keras.callbacks.Callback):
    """Print the scikit-learn classification_report after each epoch

    # Arguments
        X_val: The input values 
        Y_val: The expected output values
        classes: The categories as a list of string names
    """
    def __init__(self, X_val, Y_val, classes, normalize=False):
        self.X_val = X_val
        self.Y_val = Y_val
        self.classes = classes

    def on_train_begin(self, logs={}):
        pass
    
    def on_epoch_end(self, epoch, logs={}):   
        pred = self.model.predict(self.X_val)
        max_pred = np.argmax(pred, axis=1)
        max_y = np.argmax(self.Y_val, axis=1)
        print(classification_report(max_y, max_pred, target_names=self.classes))
```

## Einbinden der Callbacks

Im folgenden Code ist der Einsatz der Callbacks zu erkennen:

```python
EPOCHS = 30
BATCH_SIZE = 64

model = tf.keras.Sequential()
model.add(tf.keras.layers.LSTM(600, input_shape=(X_train.shape[1:]), return_sequences=True))

# .. layers

model.add(tf.keras.layers.Dense(3, activation='softmax'))

model.compile(loss='categorical_crossentropy', optimizer='adam', metrics=[
    "accuracy",
    "binary_accuracy",
    "categorical_accuracy"])

# Callbacks
plotterAccLoss = AccLossPlotter(graphs=['acc', 'loss'], save_graph=True)
plotterConfusion = ConfusionMatrixPlotter(X_val=X_val, classes=['0', '1', '2'], Y_val=y_val)
classificationReport = ClassificationReport(X_val=X_val, classes=['0', '1', '2'], Y_val=y_val)
reduce_lr = tf.keras.callbacks.ReduceLROnPlateau(monitor='val_loss', factor=0.2, patience=3, min_lr=0.00001)

model.fit(
    X_train, y_train, 
    batch_size=BATCH_SIZE, 
    epochs=EPOCHS,
    validation_data=(X_val, y_val),
    callbacks=[reduce_lr, plotterAccLoss, plotterConfusion, classificationReport])
```

## Resultate ersichtlich während der Lernphase

Im folgenden Bild ist ein Ausschnit der Trainingsphase mit Google Colab ersichtlich:
![Multiclass Training Metriken](/assets/images/multiclass-training-metrics.PNG)


<div class="divider"></div>

## Referenzen & Informationsquellen
* <a href="https://www.youtube.com/watch?v=HBi-P5j0Kec" target="_blank">Video: Performance measure on multiclass classification [accuracy, f1 score, precision, recall]</a>
* <a href="https://www.tensorflow.org/tutorials/structured_data/imbalanced_data#applying_this_tutorial_to_your_problem" target="_blank">Tensorflow: Classification on imbalanced data</a>
* <a href="https://www.curiousily.com/posts/practical-guide-to-handling-imbalanced-datasets/" target="_blank">Practical Guide to Handling Imbalanced Datasets</a>