# GalleryActivity

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
  - 이미지를 구성하는 단위

# GalleryRecyclerViewAdapter 클래스

# GalleryRecyclerViewAdapter 메소드

## Description 

- GalleryRecyclerViewAdapter 객체의 view를 생성해줍니다.
 
## Parameter

- Context context
  - 객체의 현재 상태를 나타내주는 역할을 함
  - https://velog.io/@allocproc/android-Context%EB%9E%80
    
## Return 

 - type : void
 
 - value : 없음

## Dependence function

- LayoutInflater
  - XML 파일을 해당 View 객체 로 인스턴스화 합니다. 

- LayoutInflater.from(contxt)
  - 지정된 context에서 LayoutInflater를 가져옵니다.
  - https://developer.android.com/reference/android/view/LayoutInflater#from(android.content.Context)
  
## Constructor

- new ArrayList<Uri>
  - ArrayList는 List 인터페이스를 상속받은 클래스로 크기가 가변적으로 변하는 리스트입니다.
  - Uri타입 데이터면 추가 수정 삭제 할 수 있습니다.
  - https://developer.android.com/reference/kotlin/java/util/ArrayList
 
## Sorce code

```
public GalleryRecyclerViewAdapter(Context context) {
        this.context = context;
        layoutInflater = LayoutInflater.from(context);
        itemsUri = new ArrayList<Uri>();
        galleryActivity = (GalleryActivity)context;
    }
```

# onCreateViewHolder 메소드

## Description 

- Called when RecyclerView needs a new RecyclerView.ViewHolder of the given type to represent an item.
- View들이 정리된 리스트에서 각 View를 보관하는 객체를 리턴합니다 
- https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView.Adapter#onCreateViewHolder(android.view.ViewGroup,%20int)

## Parameter

- ViewGrop parent
  - View들의 그룹
- int viewType

## Return 

- type : GalleryRecyclerViewAdapter.ItemHolder

- value :ItemHolder

## Dependence function

- layoutInflater.inflate
  - 지정된 Xml 요소들을 객체화시킵니다.
  - https://developer.android.com/reference/android/view/LayoutInflater#inflate(int,%20android.view.ViewGroup)
  
## Source code

```
@Override
    public GalleryRecyclerViewAdapter.ItemHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        CardView itemCardView = (CardView)layoutInflater.inflate(R.layout.layout_cardview, parent, false);
        return new ItemHolder(itemCardView, this);
    }
```

# onBindViewHolder메소드

## Description 

- 리스트에서 데이터를 참조하여 받아온 이미지의 위치설정

## Parameter

- GalleryRecyclerViewAdapter.ItemHolder holder
  - 리스트들의 데이터들을 보유하고있는 변수
- int position
  - 이미지의 위치
  
## Retrun
- type : void
 
- value : 없음
 
## Dependence function

- holder.setItemUri
  - 아래의 설명참조

- holder.setImageView
  - 받은 데이터를 참조하여 이미지를 지정한 위치에 위치시킨다.
  
## Source code

```
@Override
    public void onBindViewHolder(GalleryRecyclerViewAdapter.ItemHolder holder, int position) {
        Uri targetUri = itemsUri.get(position);
        holder.setItemUri(targetUri.getPath());

        if (targetUri != null) {

            try {
                holder.setImageView(Util.convertImageFileToBitmap(targetUri.getPath(), 75, 85));
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
```

# getItemCount 메소드

## Description 

- 어댑터가 보유한 데이터의 총 항목 수를 리턴합니다
  - https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView.Adapter#getItemCount()
  
## Parameter

- 없음

## Retrun
- type : void
 
- value : 없음
 
## Dependence function

- itemUri.size()
  - 리스트의 컬렉션프레임워크 타입의 길이를 알려줍니다.
  - https://developer.android.com/reference/android/util/Size
  - https://mine-it-record.tistory.com/126

- 컬렉션프레임워크란
  - 다수의 데이터를 쉽고 효과적으로 처리할 수 있는 표준화된 방법
  
## source code

```
 @Override
    public int getItemCount() {
        return itemsUri.size();
    }
```
  
# setOnItemClickListener

## Description 

- AdapterView의 항목을 클릭했을 때 호출되는 콜백 메소드
  - https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView.Adapter#getItemCount()
  
## Parameter

- OnItemClickListener listener
  - The callback that will be invoked. This value may be null.
  - https://developer.android.com/reference/android/widget/AdapterView#setOnItemClickListener(android.widget.AdapterView.OnItemClickListener)
  
## Retrun
- type : void
 
- value : 없음
 
## Source code

```
public void setOnItemClickListener(OnItemClickListener listener) {
        onItemClickListener = listener;
    }
```

# getOnItemClickListener()메소드

## Description 

- AdapterView의 항목과 함께 호출 될 콜백이 클릭되었거나 콜백이 설정되지 않은 경우 null입니다.
  - 콜백이 호출할때 발생하는 값을 리턴합니다.
  - https://developer.android.com/reference/android/widget/AdapterView#getOnItemClickListener()
  
## Parameter

- 없음

## Retrun

- type : void
 
- value : 없음
 
## Source code

```
    public OnItemClickListener getOnItemClickListener() {
        return onItemClickListener;
    }
```

# remove()메소드

## Description 

- 리스트의 데이터를 삭제합니다.

## Parameter

- int selectedIndex
  - 제거할 요소의 인덱스

## Retrun

- type : void
 
- value : 없음

## Dependence function

- itemsUri.remove()
  - 지정된 위치의 요소를 제거합니다.
  - https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView.Adapter#notifyItemRemoved(int)

- notifyItemRemoved()
  - 사용자에게 이전에 위치한 항목position이 리스트에서 제거되었음을 알립니다.
  - https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView.Adapter#notifyItemRemoved(int)
  
-  galleryActivity.showNoImagesText()
   - 리스트가 없으면 사용자에게 보여줄 이미지가 없다고 알립니다
 
## Source code

```
 public void remove(int selectedIndex) {
        itemsUri.remove(selectedIndex);
        notifyItemRemoved(selectedIndex);

        if(itemsUri.size() == 0){
            galleryActivity.showNoImagesText();
        }
    }
```

# OnItemClickListener 인터페이스

## Description 

- onItemClick메소드를 위한 OnItemClickListener인터페이스이다.

## Source code

```
  public interface OnItemClickListener {
        public void onItemClick(ItemHolder item, int position);
    }
```

# add()메소드

## Description 

- 목록의 지정된 위치에 지정된 요소를 삽입합니다.
  - https://developer.android.com/reference/java/util/ArrayList#add(int,%20E)
  
## Parameter

- int location
  - 지정된 요소가 삽입 될 인덱스
  - https://developer.android.com/reference/java/util/ArrayList#add(int,%20E)

- Uri uri
  - 삽입 할 요소
  - https://developer.android.com/reference/java/util/ArrayList#add(int,%20E)

## Retrun

- type : void
 
- value : 없음

## Dependence function

- notifyItemInserted()
  - 사용자에게 항목 position 이 새로 삽입되었음을 알립니다
  - https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView.Adapter#notifyItemInserted(int)
  - position: 데이터 리스트에서 새로 삽입된 항목의 위치
  

## Source code

```
public void add(int location, Uri iUri) {
        itemsUri.add(location, iUri);
        notifyItemInserted(location);
    }
```

# ItemHolder클래스 

# ItemHolder() 메소드

## Description 

- 변수 imageView, cardView, parent를 초기화시킨다

## Parameter
  
- CardView cardView
  - 둥근 모서리 배경과 그림자가있는 FrameLayout입니다.
  - https://developer.android.com/reference/androidx/cardview/widget/CardView?hl=en

- GalleryRecyclerViewAdapter parent

## Retrun

- type : void
 
- value : 없음

## Dependence function

- itemView.setOnClickListener(this);
  - 뷰항목이 클릭되었을때 onClick 메소드를 콜백한다.
  - https://developer.android.com/reference/android/view/View#setOnClickListener(android.view.View.OnClickListener)
  
## Source code

```
  public ItemHolder(CardView cardView, GalleryRecyclerViewAdapter parent) {
            super(cardView);
            itemView.setOnClickListener(this);
            this.cardView = cardView;
            this.parent = parent;
            imageView = (ImageView) cardView.findViewById(R.id.item_image);
        }
```

# setItemUri()메소드

## Description 

- 리스트 itemuri 배열을 String 문자열 형태로 초기화 시킵니다.

## Parameter
  
- String itemUri

## Retrun

- type : void
 
- value : 없음

## Source code

```
 public void setItemUri(String itemUri){
            this.itemUri = itemUri;
        }
```

# getItemUri()메소드

## Description 

- 문자열로 변환한 itemUri배열 리턴

## Parameter
  
- 없음

## Retrun

- type : String
 
- value : String itemUri

## Source code

```
public String getItemUri(){
            return itemUri;
        }
```

# setImageView()메소드

## Description

- 이미지뷰의 내용으로 비트맵을 설정함

## Parameter
  
- 없음

## Retrun

- type : void
 
- value : 

## Dependence function

- imageView.setImageBitmap()
  - ImageView의 내용으로 Bitmap을 설정합니다.
  - https://developer.android.com/reference/android/widget/ImageView#setImageBitmap(android.graphics.Bitmap)
  
## Source code

```
public void setImageView(Bitmap bitmap){
            imageView.setImageBitmap(bitmap);
        }
```

# onClick메소드

## Description

- 뷰항목이 클릭되면 호출됩니다.

## Parameter
  
- View v
  - 뷰클래스의 변수

## Retrun

- type : void
 
- value : 없음

## Dependence function

- parent.getOnItemClickListener()
  - AdapterView의 항목과 함께 호출 될 콜백이 클릭되었거나 콜백이 설정되지 않은 경우 null값을 호출함
  - https://developer.android.com/reference/android/widget/AdapterView#getOnItemClickListener()
  
- listener.onItemClick()
  - AdapterView의 항목을 클릭했을 때 호출되는 콜백 메서드
  - 선택한 항목과 관련된 데이터에 엑세스해야하는경우 getItemAtPosition (position)을 호출 할 수 있습니다.
  - https://developer.android.com/reference/android/widget/AdapterView.OnItemClickListener#public-methods_1
  
## Source code

```
  @Override
        public void onClick(View v) {
            final OnItemClickListener listener = parent.getOnItemClickListener();
            if(listener != null){
                listener.onItemClick(this, getLayoutPosition());
                //or use
                //listener.onItemClick(this, getAdapterPosition());
            }
```

# LandingActivity 클래스

## requsetPermissions()메소드

## Description 

 - Camera와 내부저장소의 권한을 가져옵니다.

## Parameter

- 없음
    
## Return 

 - type : void
 
 - value : 없음

## Dependence function

- ActivityCompat.requestPermissions() 
  - 퍼미션을 요청합니다
  - 파라미터는 요청 코드를 넣습니다.
  - https://developer.android.com/training/permissions/requesting?hl=ko
- ContextCompat.checkSelfPermission()
  - 앱에 특정권한이 부여됬는지 확인합니다.
  - 앱에 권한이 있는지에 따라 PERMISSION_GRANTED 또는 PERMISSION_DENIED를 반환합니다.
  - https://developer.android.com/training/permissions/requesting?hl=ko

## Source code 

```
 private void requsetPermissions(){
        if(ContextCompat.checkSelfPermission(this,Manifest.permission.CAMERA)!=PackageManager.PERMISSION_GRANTED
            || ContextCompat.checkSelfPermission(this,Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED)

            ActivityCompat.requestPermissions(this,new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE,Manifest.permission.CAMERA},REQUST_ALL_PERMISSION);

    }
```

# onRequestPermissionResult()메소드

## description 

 - requestPermissions메소드의 콜백메소드
   - https://developer.android.com/reference/androidx/core/app/ActivityCompat.OnRequestPermissionsResultCallback?hl=ko#onRequestPermissionsResult(int,%20java.lang.String[],%20int[])
            
## parameter

- int permsRequestCode 
  - 전달된 요청 코드

- @NonNull String[] permissions
  - 요청 된 권한

- @NonNull int[] grandResults
  - 해당 권한에 대한 부여 결과
  - https://developer.android.com/reference/androidx/core/app/ActivityCompat.OnRequestPermissionsResultCallback?hl=ko

## return value 

- type
  - void

- value
  - 없음
    
## Dependence function

- ActivityCompat.shouldShowRequestPermissionRationale
  - 권한을 요청하기 전에 근거가있는 UI를 표시해야하는지 여부를 가져옵니다.
  - 권한 요청을 한 번 거절하면 메서드 반환 값이 true가 됩니다.
  - https://developer.android.com/reference/androidx/core/app/ActivityCompat?hl=ko#shouldShowRequestPermissionRationale(android.app.Activity,%20java.lang.String)
  - https://academy.realm.io/kr/posts/android-marshmellow-permission/
  
## Source code

```
 @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        switch (requestCode){
            case REQUST_ALL_PERMISSION:
                for(int grantResult : grantResults){
                    if(grantResult == PackageManager.PERMISSION_DENIED){
                        Toast.makeText(this,"아직 승인받지 않았습니다.",Toast.LENGTH_LONG).show();
                    }else{
                        Toast.makeText(this,"승인이 허가되어 있습니다.",Toast.LENGTH_LONG).show();
                    }
                }
        }
    }
```

# initializeWebView()메소드

- https://github.com/jun-seok816/HybridApp 참조

# getFileNameForPicture() 메소드

## description 

- 사진파일의 이름을 현재시간 jpg로 설정합니다.

## Parameter

- 없음
    
## Return 

 - type : String
 
 - value : 없음
 
## Dependence function

- System.currentTimeMillis();
  - 현재시간을 반환합니다.
  - https://developer.android.com/reference/java/lang/System#currentTimeMillis()
  
## Source code

```
   private String getFileNameForPicture() {
        Long tsLong = System.currentTimeMillis() / 1000;
        String ts = tsLong.toString() + ".jpg";
        return ts;
    }
```


# captureImage() 메소드

## description 

- 사진파일의 이름을 현재시간 jpg로 설정합니다.

## Parameter

- 없음
    
## Return 

 - type : void
 
 - value : 없음
 
## New File()

-  이미지를 저장할 파일을 만듭니다
 
## Dependence function

- gettAbsolutePath()
  - 이 파일의 절대경로를 반환합니다
  - https://developer.android.com/reference/java/io/File#getAbsolutePath()
  
- cameraIntent.putExtra()
  - 인텐트에 확장 데이터를 추가합니다
  - https://developer.android.com/reference/android/content/Intent#putExtra(java.lang.String,%20boolean)
  
- startActivityForResult()
  - onActivityResult메소드와 콜백에서 액티비티의 결과를 별도의 Intent객체로 수신하는 함수
  - https://developer.android.com/reference/android/app/Activity?hl=ko#startActivityForResult(android.content.Intent,%20int
  
## Source code

```
public void captureImage() {
        Intent cameraIntent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);

        //Create the file for storing the image
        File directory = new File(Environment.getExternalStorageDirectory().getAbsolutePath()
                + "/WebviewCameraDemo/");
        // Create file directory if it doesn't exist
        if (!directory.exists()) {
            boolean isDirectoryCreated = directory.mkdir();
        }

        File pictureFile = new File(directory, getFileNameForPicture());
        try {
            if (!pictureFile.exists()) {
                pictureFile.createNewFile();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

        msCurrentCaptureFilePath = pictureFile.getAbsolutePath();


        cameraIntent.putExtra(MediaStore.EXTRA_OUTPUT, Uri.fromFile(pictureFile));
        startActivityForResult(cameraIntent, Constants.REQ_CAMERA);
}
```
  
# OnActivityResult메소드


## description 

- startActivityForResult메소드의 콜백 메소드
  - https://developer.android.com/reference/android/app/Activity?hl=ko#onActivityResult(int,%20int,%20android.content.Intent)
            
## parameter

- requestCode: 대화상자 액티비티랑 MainActivity를 구별하기 위한 코드

- resultCode: 어떠한 결과코드를 주었는지에 대한 변수

- Intent data: 액티비티에서 보낸 결과 데이터가 들어가있는 부분
  - https://developer.android.com/reference/android/app/Activity?hl=ko#onActivityResult(int,%20int,%20android.content.Intent)
    
## return value

- type: void

- value:없음

## new FileOutputStream

- 지정된 이름으로 파일에 쓸 파일 output Stream을 만든다
- 주어진 File 객체가 가리키는 파일을 쓰기 위한 객체를 생성.
  - https://developer.android.com/reference/java/io/FileOutputStream#FileOutputStream(java.lang.String)
  - http://blog.naver.com/PostView.nhn?blogId=eunjin6132&logNo=220888811967&categoryNo=38&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=search

## Case 

- requestCode가 Constants.REQ_CAMERA일때
  - 리소스파일을 비트맵으로 바꿔서 jpeg파일로 압축
  
- requestCode가 Constants.REQ_GALLERY일때
  - Jpeg파일을 html 사이트에 업로드한다.

## Dependence function

- Util.convertImageFileToBitmap()
  - 리소스 파일을 비트맵 으로 변환
  - Util 클래스 참조
- viewBitmap.compress()
  - Bitmap.CompressFormat: 비트맵을 JPEG형식으로 압축합니다
  - 압축 된 버전의 비트 맵을 지정된 출력 스트림에 씁니다
  - https://developer.android.com/reference/android/graphics/Bitmap#compress(android.graphics.Bitmap.CompressFormat,%20int,%20java.io.OutputStream)
- Stream
  - 순차 및 병렬 집계 작업을 지원하는 일련의 요소입니다.
  - https://developer.android.com/reference/java/util/stream/Stream
- outStream.close();
  - 파일 출력 스트림을 닫고 스트림과 관련된 모든 시스템 리소스를 해제합니다. 
  - https://developer.android.com/reference/java/io/FileOutputStream#close()

- alertClickImages.setMessage()
  - 대화상자의 메세지
  - https://developer.android.com/reference/android/app/AlertDialog.Builder#setMessage(int)

- alertClickImages.setPositiveButton()
  - 대화상자의 OK버튼을 눌렀을때 일어나는 함수

- alertClickImages.setPositiveButton()
  - 대화상자의 No 버튼을 눌렀을때 일어나는 함수
  - https://developer.android.com/reference/android/app/AlertDialog.Builder#setPositiveButton(int,%20android.content.DialogInterface.OnClickListener)

- load.url()
  - loadurl("javascript:(function(){}문구가 안에 있는 Javascript 코드를 실행시킨다.
  
## loadURL()메소드

## description 

- 안드로이드 내부에 저장한 html 파일을 웹뷰에 띄운다

## Parameter

- 없음
    
## Return 

 - type : String
 
 - value : 없음
 
## Source code

```
mWebView.loadUrl("file:///android_asset/index.html");
```

# WebVCamBridgeInterface 클래스

## Source code

```
@JavascriptInterface
        public void takePicture() {
            captureImage();

        }


        @JavascriptInterface
        public void showPictures() {
            Intent intent = new Intent(LandingActivity.this, GalleryActivity.class);
            startActivityForResult(intent, Constants.REQ_GALLERY);
        }
```

  
  






