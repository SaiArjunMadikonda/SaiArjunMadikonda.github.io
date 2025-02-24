---
name: MyDerma - Mobile Deep Learning for Skin Cancer Detection
tools: [TensorFlow Lite, ResNet-50, InceptionV3, DenseNet201, Flutter]
image: https://raw.githubusercontent.com/vishnumandala/MyDerma-Mobile-Deep-Learning-for-Real-Time-Skin-Cancer-Detection/main/results/demo.gif
description: Mobile application using ensemble deep learning models for real-time skin cancer detection with 97.15% accuracy.
---

<div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">
    <h1 style="margin: 0;"><strong>MyDerma - Mobile Deep Learning for Skin Cancer Detection</strong></h1>
    <a href="https://github.com/vishnumandala/MyDerma-Mobile-Deep-Learning-for-Real-Time-Skin-Cancer-Detection" 
       style="text-decoration: none; background-color: #f5f5f5; padding: 10px 15px; border-radius: 8px; transition: all 0.3s ease;">
        <i class="fab fa-github fa-2x" style="color: #333333; transition: color 0.3s ease;"></i>
        <style>
            a:hover {
                background-color: #333333 !important;
            }
            a:hover i {
                color: #ffffff !important;
            }
        </style>
    </a>
</div>

<p class="post-metadata text-muted">
   <span class="d-inline-block">June 25, 2023</span> &#8226; 
   <span class="tags">
      {% for tag in page.tools %}
      <span class="tag badge badge-pill text-primary border border-primary">{{ tag }}</span>
      {% endfor %}
    </span>
</p>

<div style="text-align: center; margin: 30px 0;">
    <img src="https://raw.githubusercontent.com/vishnumandala/MyDerma-Mobile-Deep-Learning-for-Real-Time-Skin-Cancer-Detection/main/results/demo.gif" 
         alt="MyDerma App Demo"
         style="width: 90%; max-width: 1200px; margin: auto;"
    />
    <p style="margin-top: 10px; font-style: italic; color: #666;">MyDerma Mobile Application Demo</p>
</div>

## Project Overview

A mobile application leveraging ensemble deep learning models for real-time skin cancer detection. The system combines multiple state-of-the-art architectures (ResNet-50, InceptionV3, DenseNet201) optimized for mobile deployment, achieving high accuracy while maintaining real-time performance.

## Key Features

- **Ensemble Architecture**: Combined ResNet-50, InceptionV3, and DenseNet201
- **Mobile Optimization**: TensorFlow Lite deployment with quantization
- **Real-time Processing**: Under 2 seconds inference time
- **High Accuracy**: 97.15% detection accuracy
- **User-friendly Interface**: Intuitive Flutter-based UI

## Technical Architecture

### Model Architecture
```python
class EnsembleModel(tf.keras.Model):
    def __init__(self):
        super(EnsembleModel, self).__init__()
        self.resnet = ResNet50(weights='imagenet', include_top=False)
        self.inception = InceptionV3(weights='imagenet', include_top=False)
        self.densenet = DenseNet201(weights='imagenet', include_top=False)
        
        # Custom layers
        self.global_pool = GlobalAveragePooling2D()
        self.dropout = Dropout(0.5)
        self.classifier = Dense(7, activation='softmax')
        
    def call(self, inputs):
        # Process through each model
        x1 = self.resnet(inputs)
        x2 = self.inception(inputs)
        x3 = self.densenet(inputs)
        
        # Combine features
        x = Concatenate()([
            self.global_pool(x1),
            self.global_pool(x2),
            self.global_pool(x3)
        ])
        
        x = self.dropout(x)
        return self.classifier(x)
```

## Implementation Details

### Mobile Optimization
```python
def optimize_for_mobile(model):
    # Convert to TFLite
    converter = tf.lite.TFLiteConverter.from_keras_model(model)
    
    # Enable optimizations
    converter.optimizations = [tf.lite.Optimize.DEFAULT]
    converter.target_spec.supported_types = [tf.float16]
    
    # Quantization
    converter.target_spec.supported_ops = [
        tf.lite.OpsSet.TFLITE_BUILTINS,
        tf.lite.OpsSet.SELECT_TF_OPS
    ]
    
    # Convert and save
    tflite_model = converter.convert()
    return tflite_model
```

## Performance Metrics

### System Performance
- **Detection Accuracy**: 97.15% on test set
- **Inference Time**: < 2 seconds on mobile
- **Model Size**: Reduced from 300MB to 50MB
- **False Positive Rate**: < 2%

## Future Development

Potential areas for future enhancement include:
- Integration with medical databases
- Real-time lesion segmentation
- Automated report generation
- Telemedicine integration 