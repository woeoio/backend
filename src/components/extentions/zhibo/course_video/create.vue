<style lang="less"></style>
<template>
  <div class="h-panel w-1000">
    <div class="h-panel-bar">
      <span class="h-panel-title">添加</span>
    </div>
    <div class="h-panel-body">
      <Form
        mode="block"
        ref="form"
        :validOnChange="true"
        :showErrorTip="true"
        :rules="rules"
        :model="video"
      >
        <Row>
          <Cell :width="24">
            <FormItem label="标题" prop="title">
              <input type="text" v-model="video.title" />
            </FormItem>
          </Cell>
        </Row>
        <Row :space="10">
          <Cell :width="6">
            <FormItem label="课程" prop="course_id">
              <Select
                v-model="video.course_id"
                :datas="courses"
                keyName="id"
                titleName="title"
                :filterable="true"
              ></Select>
            </FormItem>
          </Cell>
          <Cell :width="6">
            <FormItem label="章节" prop="chapter_id">
              <Select
                v-model="video.chapter_id"
                :datas="chapters[video.course_id]"
                keyName="id"
                titleName="name"
                :filterable="true"
              ></Select>
            </FormItem>
          </Cell>
          <Cell :width="6">
            <FormItem label="直播时间" prop="published_at">
              <DatePicker v-model="video.published_at" v-width="200" type="datetime"></DatePicker>
            </FormItem>
          </Cell>
          <Cell :width="2"></Cell>
          <Cell :width="4">
            <FormItem label="是否显示" prop="is_show">
              <h-switch v-model="video.is_show" :trueValue="1" :falseValue="0"></h-switch>
            </FormItem>
          </Cell>
        </Row>

        <FormItem>
          <Button color="primary" @click="create">添加</Button>
        </FormItem>
      </Form>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      video: {
        course_id: null,
        chapter_id: null,
        title: '',
        published_at: '',
        is_show: 1
      },
      rules: {
        required: ['course_id', 'chapter_id', 'title', 'published_at', 'is_show']
      },
      courses: [],
      chapters: []
    };
  },
  mounted() {
    R.Extentions.zhibo.CourseVideo.Create().then(res => {
      this.courses = res.data.courses;
      this.chapters = res.data.chapters;
    });
  },
  methods: {
    create() {
      let validResult = this.$refs.form.valid();
      if (validResult.result) {
        R.Extentions.zhibo.CourseVideo.Store(this.video).then(resp => {
          HeyUI.$Message.success('成功');
          this.$emit('success');
        });
      }
    }
  }
};
</script>
