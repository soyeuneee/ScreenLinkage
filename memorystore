package com.example.memorystore

import android.Manifest
import android.annotation.SuppressLint
import android.app.Activity
import android.content.Intent
import android.graphics.Bitmap
import android.graphics.Color
import android.os.Bundle
import android.provider.MediaStore
import android.widget.Button
import android.widget.EditText
import android.widget.ImageView
import android.widget.LinearLayout
import android.widget.TextView
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import androidx.constraintlayout.widget.ConstraintLayout

class DiaryActivity : AppCompatActivity() {

    private lateinit var imageViewDiary: ImageView
    private lateinit var editTextMemory: EditText
    private lateinit var buttonCamera: Button
    private lateinit var buttonGallery: Button
    private lateinit var buttonSave: Button

    @SuppressLint("MissingInflatedId")
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        // XML 레이아웃 직접 정의
        val constraintLayout = ConstraintLayout(this).apply {
            layoutParams = ConstraintLayout.LayoutParams(
                ConstraintLayout.LayoutParams.MATCH_PARENT,
                ConstraintLayout.LayoutParams.MATCH_PARENT
            )
            setBackgroundColor(android.graphics.Color.parseColor("#FFF4F7"))
        }

        val titleTextView = TextView(this).apply {
            id = R.id.titleTextView
            text = "오늘의 다이어리"
            textSize = 20f
            setTextColor(android.graphics.Color.BLACK)
            layoutParams = ConstraintLayout.LayoutParams(
                ConstraintLayout.LayoutParams.WRAP_CONTENT,
                ConstraintLayout.LayoutParams.WRAP_CONTENT
            ).apply {
                setMargins(16, 16, 16, 0)
            }
        }

        imageViewDiary = ImageView(this).apply {
            id = R.id.eimageViewDiary
            layoutParams = ConstraintLayout.LayoutParams(
                ConstraintLayout.LayoutParams.MATCH_PARENT,
                200
            ).apply {
                setMargins(16, 16, 16, 16)
            }
            setBackgroundColor(Color.parseColor("#FFF4F7"))
            scaleType = ImageView.ScaleType.CENTER_CROP
        }

        constraintLayout.addView(imageViewDiary)


        val buttonLayout = LinearLayout(this).apply {
            orientation = LinearLayout.HORIZONTAL
            layoutParams = ConstraintLayout.LayoutParams(
                ConstraintLayout.LayoutParams.WRAP_CONTENT,
                ConstraintLayout.LayoutParams.WRAP_CONTENT
            )
        }

        buttonCamera = Button(this).apply {
            id = R.id.buttonCamera
            text = "카메라"
        }

        buttonGallery = Button(this).apply {
            id = R.id.buttonGallery
            text = "갤러리"
        }

        buttonLayout.addView(buttonCamera)
        buttonLayout.addView(buttonGallery)

        editTextMemory = EditText(this).apply {
            id = R.id.editTextMemory
            hint = "연인과의 소중한 추억을 남겨 주세요!"
            setBackgroundColor(android.graphics.Color.parseColor("#F8F8F8"))
            layoutParams = ConstraintLayout.LayoutParams(
                ConstraintLayout.LayoutParams.MATCH_PARENT,
                ConstraintLayout.LayoutParams.WRAP_CONTENT
            )
        }

        buttonSave = Button(this).apply {
            id = R.id.buttonSave
            text = "저장"
            setBackgroundColor(android.graphics.Color.parseColor("#F06292"))
            setTextColor(android.graphics.Color.WHITE)
        }

        constraintLayout.addView(titleTextView)
        constraintLayout.addView(imageViewDiary)
        constraintLayout.addView(buttonLayout)
        constraintLayout.addView(editTextMemory)
        constraintLayout.addView(buttonSave)

        setContentView(constraintLayout)

        // Constraint 연결
        val set = androidx.constraintlayout.widget.ConstraintSet()
        set.clone(constraintLayout)

        set.connect(titleTextView.id, ConstraintLayout.LayoutParams.TOP, ConstraintLayout.LayoutParams.PARENT_ID, ConstraintLayout.LayoutParams.TOP, 16)
        set.connect(titleTextView.id, ConstraintLayout.LayoutParams.START, ConstraintLayout.LayoutParams.PARENT_ID, ConstraintLayout.LayoutParams.START, 16)

        set.connect(imageViewDiary.id, ConstraintLayout.LayoutParams.TOP, titleTextView.id, ConstraintLayout.LayoutParams.BOTTOM, 16)
        set.connect(imageViewDiary.id, ConstraintLayout.LayoutParams.START, ConstraintLayout.LayoutParams.PARENT_ID, ConstraintLayout.LayoutParams.START, 16)
        set.connect(imageViewDiary.id, ConstraintLayout.LayoutParams.END, ConstraintLayout.LayoutParams.PARENT_ID, ConstraintLayout.LayoutParams.END, 16)

        set.connect(buttonLayout.id, ConstraintLayout.LayoutParams.TOP, imageViewDiary.id, ConstraintLayout.LayoutParams.BOTTOM, 16)
        set.connect(buttonLayout.id, ConstraintLayout.LayoutParams.START, ConstraintLayout.LayoutParams.PARENT_ID, ConstraintLayout.LayoutParams.START, 16)

        set.connect(editTextMemory.id, ConstraintLayout.LayoutParams.TOP, buttonLayout.id, ConstraintLayout.LayoutParams.BOTTOM, 16)
        set.connect(editTextMemory.id, ConstraintLayout.LayoutParams.START, ConstraintLayout.LayoutParams.PARENT_ID, ConstraintLayout.LayoutParams.START, 16)
        set.connect(editTextMemory.id, ConstraintLayout.LayoutParams.END, ConstraintLayout.LayoutParams.PARENT_ID, ConstraintLayout.LayoutParams.END, 16)

        set.connect(buttonSave.id, ConstraintLayout.LayoutParams.TOP, editTextMemory.id, ConstraintLayout.LayoutParams.BOTTOM, 16)
        set.connect(buttonSave.id, ConstraintLayout.LayoutParams.START, ConstraintLayout.LayoutParams.PARENT_ID, ConstraintLayout.LayoutParams.START, 16)
        set.connect(buttonSave.id, ConstraintLayout.LayoutParams.END, ConstraintLayout.LayoutParams.PARENT_ID, ConstraintLayout.LayoutParams.END, 16)

        set.applyTo(constraintLayout)

        // 버튼 이벤트 처리
        buttonCamera.setOnClickListener {
            val cameraIntent = Intent(MediaStore.ACTION_IMAGE_CAPTURE)
            startActivityForResult(cameraIntent, REQUEST_CAMERA)
        }

        buttonGallery.setOnClickListener {
            val galleryIntent = Intent(Intent.ACTION_PICK, MediaStore.Images.Media.EXTERNAL_CONTENT_URI)
            startActivityForResult(galleryIntent, REQUEST_GALLERY)
        }

        buttonSave.setOnClickListener {
            val memoryText = editTextMemory.text.toString()
            if (memoryText.isEmpty()) {
                Toast.makeText(this, "내용을 입력해 주세요!", Toast.LENGTH_SHORT).show()
            } else {
                Toast.makeText(this, "저장되었습니다!", Toast.LENGTH_SHORT).show()
                // 저장 로직 추가
            }
        }
    }

    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)
        if (resultCode == Activity.RESULT_OK) {
            when (requestCode) {
                REQUEST_CAMERA -> {
                    val imageBitmap = data?.extras?.get("data") as? Bitmap
                    imageViewDiary.setImageBitmap(imageBitmap)
                }
                REQUEST_GALLERY -> {
                    val selectedImageUri = data?.data
                    imageViewDiary.setImageURI(selectedImageUri)
                }
            }
        }
    }

    companion object {
        private const val REQUEST_CAMERA = 100
        private const val REQUEST_GALLERY = 101
    }
}
