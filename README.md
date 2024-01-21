import android.os.AsyncTask
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import kotlinx.android.synthetic.main.activity_main.*
import org.jsoup.Jsoup

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // Запускаем асинхронную задачу для загрузки и парсинга информации
        LoadDataTask().execute()
    }

    private inner class LoadDataTask : AsyncTask<Void, Void, String>() {

        override fun doInBackground(vararg params: Void): String {
            val url = "https://fakelfc.ru/" // Замените на реальную ссылку на официальный сайт

            return try {
                // Используем Jsoup для загрузки страницы
                val doc = Jsoup.connect(url).get()

                // Парсим необходимую информацию (например, описание клуба)
                val description = doc.select("div.description").first().text()

                description
            } catch (e: Exception) {
                e.printStackTrace()
                "Ошибка при загрузке информации"
            }
        }

        override fun onPostExecute(result: String) {
            super.onPostExecute(result)

            // Отображаем информацию на экране
            clubTextView.text = result
        }
    }
}
