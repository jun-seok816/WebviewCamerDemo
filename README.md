# setUpload()메소드

## Description 

 - 업로드할 사진을 설정합니다.

## Parameter

- 없음
    
## Return 

 - type : void
 
 - value : 없음

## Dependence function

- intent.putExtra()
  - 인텐트에 확장 데이터를 추가합니다.
  - 지정된 문자열에 데이터를 넣습니다.
  - https://developer.android.com/reference/android/content/Intent#putExtra(java.lang.String,%20boolean)
  
- setResult()
  - Activity에서 돌려받을 결과값을 설정합니다.
  - https://developer.android.com/reference/android/app/Activity#setResult(int,%20android.content.Intent)
  

## Source code 

```
private void setUpload() {
        findViewById(R.id.btn_upload).setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View view) {
                if(selectedIndex == -1){
                    Toast.makeText(GalleryActivity.this, getString(R.string.please_select_image_to_upload), Toast.LENGTH_SHORT).show();
                    return;
                }
                Intent intent = new Intent();
                intent.putExtra(Constants.PATH_OF_IMAGE_TO_UPLOAD, selectedImage);
                setResult(202, intent);
                selectedImage = null;
                selectedIndex = -1;
                finish();
            }
        });
    }
```

# setDelete()메소드

## Description 

 - 삭제할 사진을 설정합니다.

## Parameter

- 없음
    
## Return 

 - type : void
 
 - value : 없음
 
## Constructor

- AlertDialog.Builder
  - 기본 경고 대화 상자 테마를 사용하는 경고 대화 상자의 빌더를 만듭니다.
  - https://developer.android.com/reference/android/app/AlertDialog.Builder

## Dependence function

- alertClickImages.setPositiveButton()
  - 대화 상자의 Ok 버튼을 누를 때 호출 할 리스너를 설정합니다.
  
- imageView.setImageDrawable(null) 
  - Drawable 화면에 그릴 수 있는 그래픽의 일반적인 개념
  - Drawable의 내용을 지우기위해 null값을 넣음
  - https://developer.android.com/reference/android/widget/ImageView#setImageDrawable(android.graphics.drawable.Drawable)
- alertClickImages.setNegativeButton()
  - 대화 상자의 No 버튼을 누를 때 호출 할 리스너를 설정합니다.

## Source code

```
 private void setDelete() {
        findViewById(R.id.btn_delete).setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View view) {
                if(selectedIndex == -1){
                    Toast.makeText(GalleryActivity.this, getString(R.string.please_select_image_to_delete), Toast.LENGTH_SHORT).show();
                    return;
                }
                // Display alert dialog to confirm delete
                AlertDialog.Builder alertClickImages = new AlertDialog.Builder(GalleryActivity.this);
                alertClickImages.setMessage(getString(R.string.are_you_sure_to_delete));
                alertClickImages.setCancelable(true);

                alertClickImages.setPositiveButton(
                        "Yes",
                        new DialogInterface.OnClickListener() {
                            public void onClick(DialogInterface dialog, int id) {
                                File file = new File(selectedImage);
                                boolean isDelete = file.delete();

                                recyclerViewAdapter.remove(selectedIndex);
                                ImageView imageView = (ImageView) findViewById(R.id.image_preview);
                                imageView.setImageDrawable(null);
                                dialog.cancel();
                                selectedImage = null;
                                selectedIndex = -1;
                            }
                        });

                alertClickImages.setNegativeButton(
                        "No",
                        new DialogInterface.OnClickListener() {
                            public void onClick(DialogInterface dialog, int id) {
                                dialog.cancel();
                            }
                        });
                alertClickImages.show();
            }
        });
    }
```

# setDelete()메소드

## Description 

 - 삭제할 사진을 설정합니다.

## Parameter

- 없음
    
## Return 

 - type : void
 
 - value : 없음
 
## Constructor

- new File()
  - 지정된 인스턴스를 추상화된 파일의 경로로 설정한다
  - https://developer.android.com/reference/java/io/File

## Dependence function

- targetDirector.listFiles()
  - targetDirector의 디렉토리의 파일을 표시하는 추상 경로명의 배열을 리턴합니다.
  - https://developer.android.com/reference/java/io/File#listFiles()
  
- Uri.fromeFile()
  - 파일에서 Uri를 만듭니다.
  - https://developer.android.com/reference/android/net/Uri#fromFile(java.io.File)

- recyclerViewAdapter.getItemCount()
  - Adapter의 총 항목 수를 리턴합니다.
  - https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView.Adapter#getItemCount()
  - Adapter란
  - 두 개의 상이한 부분을 연결시키는 데 사용하는 장치를 의미한다.
  - 데이터 항목들의 엑세스를 제공합니다.
  - http://blog.naver.com/PostView.nhn?blogId=kookh1&logNo=120170877682
  - https://developer.android.com/reference/android/widget/Adapter


# showNoImagesText()메소드

## Description 

 - 이미지를 찾지 못했을때 사용자에게 Text문자로 찾지 못했다고 알려줍니다.
 
## Parameter

- 없음
    
## Return 

 - type : void
 
 - value : 없음

## Dependence function

- findViewById(R.id.txt_no_images_found).setVisibility(View.VISIBLE);
  - findViewById(R.id.txt_no_images_found)의 가시성 상태를 VISIBLE로 합니다.
- findViewById(R.id.image_preview).setVisibility(View.GONE);
  - findViewById(R.id.image_preview)의 가시성 상태를 GONE로 합니다.
  
# onItemClick()메소드

## Description 

- AdapterView의 항목을 클릭했을 때 호출되는 콜백 메서드입니다.
  - https://developer.android.com/reference/android/widget/AdapterView

## Parameter

- AdapterView
  - 클릭이 발생한 AdapterView입니다.
  
- position  
  - 어댑터에서 view의 위치입니다.
  - https://developer.android.com/reference/android/widget/AdapterView.OnItemClickListener#public-methods_1

## Return 

 - type : void
 
 - value : 없음

## Dependence function

- item.getItemUri()
  - 아래에 설명 참조
- Util.convertImageFileToBitmap() 
  - Util클래스설명 참조
- imageView.setImageBitmap()  
  - imageView의 내용으로 Bitmap을 설정합니다.
- Bitmap  
  - 



  
  

